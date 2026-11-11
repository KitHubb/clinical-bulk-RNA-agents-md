# 2. 데이터 처리

## 입력 자료

가능하면 다음 파일을 사용한다.

- integer raw count matrix
- sample metadata
- gene annotation 및 ID 매핑표
- 마크로젠 QC/DEG 결과와 method 문서

DESeq2 입력에는 FPKM, TPM, CPM 또는 임의로 반올림한 정규화 값을 사용하지 않는다. DESeq2에는 음수가 없는 정수형 raw count를 사용한다.

## 입력 검증

- count matrix의 열과 metadata의 sample ID가 정확히 대응하는지 확인한다.
- sample ID 중복, gene ID 중복, 결측 group, 0 library size를 검사한다.
- 표본 순서가 달라도 ID 기준으로 명시적으로 정렬한다.
- phenotype/group 명칭의 공백, 대소문자, 오탈자를 정리한다.
- Ensembl ID에 version suffix가 있으면 제거 여부와 원래 ID를 모두 보존한다.
- count 생성 도구, reference genome, GTF/annotation release와 strandedness를 실제 파일 또는 method에서 확인한다.
- featureCounts/HTSeq integer count인지 Salmon/tximport 계열 estimated count인지 확인하고 입력 경로를 임의로 가정하지 않는다.

## PROJECT_INFO 및 design 검증 gate

DESeq2 객체 생성 전에 연구자에게 질문하고 실제 metadata로 검증하여 `PROJECT_INFO.md`에 기록한다.

- biological unit가 환자, 조직, library 중 무엇인지
- biological replicate와 technical replicate의 구분
- 각 조건의 독립 biological replicate 수
- paired 또는 repeated-measure 여부와 patient/subject ID
- 비교군, baseline 및 양의 log2FC 방향
- sequencing/library batch, 기관, RIN, sex, age, medication, stage 등 후보 공변량
- 사전 정의된 포함·제외 및 outlier 기준
- pooled 분석에 포함되는 임상 subgroup와 interaction/층화 필요성
- technical replicate가 동일 biological sample 및 동일 library의 반복 sequencing인지

다음 항목은 질문만 하지 말고 데이터로 직접 검증한다.

- `table(group, batch)` 등 교차표로 group과 batch의 완전 confounding 여부 확인
- paired design에서 subject별 조건 조합과 누락 표본 확인
- design formula에 포함된 factor의 빈 level과 singleton level 확인
- `model.matrix()`의 열 수와 `qr()` rank를 비교하여 full-rank 여부 확인
- 표본 수 대비 coefficient 수를 확인하여 과도한 모형 복잡성 점검
- biological replicate가 없는 비교는 DESeq2 추론 분석을 진행하지 않음
- 완전히 confounded된 변수는 통계적으로 분리할 수 없으므로 임의 보정하지 않고 연구자에게 보고

## DESeq2 입력과 사전 필터

DESeq2의 주 입력에는 row-sum 기준으로 필터링하지 않은 전체 raw integer count를 사용한다.

```text
countData <- full_raw_integer_count_matrix
```

- `rowSums(counts) > 0`을 포함한 외부 사전 필터를 기본으로 적용하지 않는다.
- DESeq2 `results()`의 independent filtering은 DESeq2 내부 결과 처리이며 외부 row-sum 필터와 구분한다.
- all-zero row, low-count row와 `NA` 결과의 수는 보고할 수 있지만 사용자의 지시 없이 제거한 별도 분석을 추가하지 않는다.
- 마크로젠의 필터 방식은 method 참고자료로 기록하되 현재 주 입력 규칙을 임의로 변경하지 않는다.

## DESeq2 분석

- design formula는 metadata 및 연구 설계에 따라 명시적으로 작성한다.
- batch나 paired 구조가 있으면 확인 후 design에 포함한다.
- 기준군 factor level을 명시하고 contrast를 코드에 직접 적는다.
- 분석 전에 비교군과 baseline을 사용자에게 확인한다.
- 양의 log2FC가 비교군 증가를 뜻하도록 contrast 방향을 확인하고 `PROJECT_INFO.md`에 기록한다.
- 주 DEG 기준은 BH FDR `< 0.05`로 고정하고 `results(..., alpha=0.05)`를 명시한다. 기본값에 조용히 의존하지 않는다.
- raw p-value는 `pvalue`, BH 보정값은 `padj`로 보존한다.
- 독립 필터링과 Cook's distance에 의해 `NA`가 발생한 경우 그 의미와 개수를 보고한다.
- log2FC shrinkage 방법과 coefficient를 기록한다. ORA의 효과크기 cutoff 및 방향에는 shrunken log2FC를 사용하고 p-value/padj는 원 DESeq2 결과를 사용한다.
- GSEA rank에는 shrunken log2FC가 아니라 원 DESeq2의 finite Wald statistic 전체를 사용한다.

## 변환값과 발현량

- PCA와 sample-distance/전체 heatmap에는 DESeq2 VST 또는 rlog를 우선 사용한다.
- 표본 수가 많으면 계산 효율상 VST를 기본으로 한다.
- VST/rlog 또는 별도 normalized count matrix는 파일로 저장하지 않고 실행 중 시각화에만 사용한다.
- heatmap의 row scaling 여부를 caption과 코드에 명시한다.
- CPM/FPKM/TPM을 표시해야 하면 산출 공식을 기록하고, 차등발현 검정에는 사용하지 않는다.
- 서로 다른 발현 단위를 한 figure나 표에서 혼합하지 않는다.

## ID 매핑

- enrichment에 사용하는 key type을 명시한다. 예: ENSEMBL, ENTREZID, SYMBOL.
- count 생성 annotation의 stable ID를 분석 기준으로 유지하고 gene symbol만 현재 공식 annotation으로 갱신한다.
- Count annotation release와 symbol 갱신 annotation release를 별도 기록한다.
- one-to-many 및 many-to-one 매핑 수를 보고한다.
- 중복 symbol을 단순히 첫 행으로 제거하지 않는다. 목적에 맞는 대표 ID 선택 규칙을 기록한다.
- ORA의 background는 현재 DESeq2 분석 결과에서 검정 가능하고 유효한 pathway ID로 매핑된 전체 유전자 집합을 기본으로 한다.

## 저장 산출물

- 사용자가 요청한 DESeq2 전체 결과 파일
- annotation과 ID 매핑 상태를 포함한 통합 결과
- contrast와 design formula 기록
- R 버전, package version 및 `sessionInfo()`

별도의 normalized count/VST/rlog 파일과 불필요한 중간 TSV는 저장하지 않는다. 최종 표, 그림, HTML과 session information은 `result/날짜_분석명/`에 함께 저장한다. RDS가 필요하면 `data/process/`, 재현 스크립트는 `analysis/old/`에 저장한다.

### DESeqDataSet RDS 체크포인트

다음 두 객체를 `data/process/`에 저장한다. 기존 파일은 재분석 또는 수정 요청 없이 덮어쓰지 않는다.

1. `dds_input_날짜_분석명.rds`
   - count와 metadata의 sample 순서를 확정한 후
   - factor level, baseline과 design formula를 설정한 후
   - `DESeq()` 실행 전
   - row-sum 사전 필터를 적용하지 않은 전체 raw count 포함
2. `dds_fitted_날짜_분석명.rds`
   - `DESeq()`가 오류 없이 완료된 후
   - size factor, dispersion과 fitted model 포함
   - `resultsNames(dds)` 및 model convergence를 확인한 후

VST/rlog matrix와 별도의 normalized-count 파일은 저장하지 않는다. Contrast별 `DESeqResults`는 `dds_fitted`에서 재생성하고 요청된 최종 결과표로 저장한다.

## 완료 조건

- 모든 표본과 gene ID가 추적 가능하다.
- 전체 raw count가 row-sum 사전 필터 없이 DESeq2에 입력되었음을 확인했다.
- `PROJECT_INFO.md` 필수 design 항목과 full-rank 검증이 완료되었다.
- contrast 방향을 한두 개 대표 유전자로 점검했다.
- `dds_input`과 `dds_fitted` RDS의 존재, 객체 class, sample 수와 gene 수를 다시 읽어 검증했다.
- downstream 분석에 사용할 DEG 표, ranked list, universe가 생성 가능하다.
