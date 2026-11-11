# 임상 bulk RNA-seq downstream 분석 공통 지침

## 절대 위반 금지 체크리스트

아래 항목은 모든 단계별 지침보다 우선한다. 작업 시작 전과 완료 보고 전에 각각 확인한다.

- [ ] 애매한 조건, 비교군, baseline, 저장 대상은 임의로 정하지 않고 사용자에게 먼저 질문했다.
- [ ] “확인” 요청에는 말로만 답하지 않고 실제 파일·코드·결과를 직접 열어 검사했다.
- [ ] 표 요청인지 그림 요청인지 구분했다. “GSEA 저장”처럼 curve와 결과표가 불명확하면 먼저 질문했다.
- [ ] 참고 코드나 그림이 지정되면 크기·색·폰트·배치·파일 형식을 직접 확인하고 그대로 따랐다.
- [ ] 사용자가 요청하지 않은 디자인 변경, 추가 분석 또는 범위 확장을 하지 않았다.
- [ ] 단순 추출 작업에 불필요한 스크립트·문서·검증 파일이나 다수의 중간 TSV를 만들지 않았다.
- [ ] 원자료와 기존 사용자 변경사항을 수정·삭제·되돌리지 않았다.
- [ ] 기존 결과를 덮어쓰는 작업은 사용자가 재분석 또는 수정을 요청한 경우에만 수행했다.
- [ ] 결과는 `result/날짜_분석명/` 한 폴더에 저장했고 그 아래에 `figures/`, `outputs/` 같은 하위 폴더를 만들지 않았다.
- [ ] RDS는 `data/process/`, 재현 스크립트는 `analysis/old/`에 저장했다.
- [ ] R 4.5.x, 현재 기준 R 4.5.2를 사용했으며 `renv`와 `renv.lock`을 만들지 않았다.
- [ ] 사용자 승인 없이 R 패키지를 설치·업데이트·삭제하지 않았고 기존 설치 패키지만 사용했다.
- [ ] 분석 전 비교군과 baseline을 사용자에게 확인하고 `PROJECT_INFO.md`에 기록했다.
- [ ] `PROJECT_INFO.md`를 실제 metadata 및 연구자 답변으로 작성·확인하고, 해당 분석 단계 문서와 함께 읽었다.
- [ ] biological replicate, technical replicate, paired/repeated structure, batch confounding과 design matrix full rank 여부를 검증했다.
- [ ] DESeq2에는 row-sum 사전 필터를 적용하지 않은 전체 raw integer count를 입력했다.
- [ ] GSEA에는 일부 DEG가 아니라 유한한 값인 DESeq2 Wald statistic 전체를 사용했다.
- [ ] 별도의 normalization 결과 파일을 저장하지 않았다.
- [ ] annotation 버전을 기록했다.
- [ ] count 생성 annotation과 pathway annotation을 구분하고 stable ID 기준 매핑 및 최신 symbol 표시 원칙을 적용했다.
- [ ] 완료 보고 전 산출물의 존재, 개수, 파일 크기, 내용, 오류 여부와 session information을 실제로 검증했다.
- [ ] Git commit/push는 사용자가 요청한 경우에만 수행했고, 수행 여부를 최종 답변에 명시했다.
- [ ] 잘못 이해한 경우 변명하지 않고 잘못된 지점과 수정 내용을 정확히 밝혔다.

## 1. 가장 중요한 행동 원칙

- 애매한 조건을 임의로 정하지 말고 먼저 질문한다.
- “확인” 요청에는 실제 파일·코드·결과를 직접 검사하여 답한다.
- 완료했다고 말하기 전에 파일 존재, 개수, 크기, 내용과 오류를 검증한다.
- 기존 코드나 그림을 참고하라는 요청에는 크기·색·폰트·배치·파일 형식을 직접 확인하고 그대로 따른다.
- 사용자가 요청하지 않은 디자인 변경이나 추가 분석을 임의로 넣지 않는다.
- 단순 추출 작업을 과도하게 확장하지 않는다.
- 잘못 이해했을 때 무엇을 잘못했는지 정확히 밝히고 바로 수정한다.

## 2. 프로젝트 위치와 Git

- Git pull 전에 현재 저장소, branch, remote와 변경사항을 확인한다.
- commit과 push는 사용자가 요청한 경우에만 수행한다.
- 기존 사용자 변경사항을 되돌리지 않는다.
- 원자료와 RDS의 Git 추적 여부는 기존 `.gitignore` 및 저장소 규칙을 따른다. 임의로 추적 대상을 변경하지 않는다.

## 3. 폴더와 저장 규칙

- 모든 분석 결과는 `result/날짜_분석명/`에 저장한다.
- 해당 결과 폴더 아래에는 `figures/`, `outputs/`, `tables/`, `logs/` 같은 하위 폴더를 만들지 않는다.
- 그림, HTML, 최종 표, session information은 동일한 날짜 결과 폴더에 저장한다.
- RDS는 `data/process/`에 저장한다.
- 재현 스크립트는 `analysis/old/`에 저장한다.
- 기존 결과 덮어쓰기는 사용자가 재분석 또는 수정을 요청했을 때만 허용한다.
- 불필요한 중간 TSV를 여러 개 만들지 않는다. 요청된 전체 결과 파일 하나를 우선 유지한다.

## 4. R 환경

- R은 4.5.x, 현재 기준 R 4.5.2를 사용한다.
- `renv`를 사용하지 않고 `renv.lock`을 만들지 않는다.
- 기존에 설치된 패키지만 사용한다.
- 사용자 승인 없이 패키지를 설치·업데이트·삭제하지 않는다.
- 패키지가 없으면 설치를 시작하지 말고 즉시 사용자에게 알린다.
- 실행 후 실제 R 버전, package version과 session information을 확인하고 결과 폴더에 저장한다.

## 5. RNA-seq 분석 조건

- 전체 raw count는 row sum 기준으로 사전 필터링되지 않은 값이며, 이 전체 raw integer count를 DESeq2의 주 입력값으로 사용한다.
- `rowSums(counts) > 0` 같은 사전 제거를 기본 단계로 적용하지 않는다.
- DESeq2의 내부 처리 및 `results()` independent filtering과 사용자가 요청하지 않은 외부 사전 필터를 구분한다.
- 시각화 및 분석 전에 비교군과 baseline을 사용자에게 묻고, 확정 내용을 `PROJECT_INFO.md`에 기록한다.
- biological unit, replicate 종류, paired/repeated structure, batch confounding, 포함·제외 기준과 공변량은 `PROJECT_INFO.md`에서 먼저 확정한다.
- GSEA는 일부 DEG가 아니라 NA/Inf가 아닌 DESeq2 Wald statistic 전체를 사용한다.
- 주 DEG 기준은 BH FDR `< 0.05`로 고정하고 DESeq2 결과는 `results(..., alpha=0.05)`로 생성한다.
- 별도의 normalized count, VST 또는 rlog matrix 결과 파일을 저장하지 않는다. 시각화에 필요하면 실행 중 생성하고 최종 figure만 저장한다.
- 전처리 후 `DESeqDataSet`과 DESeq2 model fitting 후 `DESeqDataSet`은 단계별 중간 RDS로 `data/process/`에 저장한다. 세부 저장 시점과 파일명은 `AGENTS_02_data_processing.md`를 따른다.

## 6. DEG·ORA·GSEA 기준

마크로젠 기준은 sensitivity/reproduction 분석의 참고자료이며 정답으로 간주하지 않는다.

- DEG 표시 기준: BH FDR `< 0.05` 및 `|log2FC| > 1`.
- ORA 주 분석의 통계 기준: BH FDR `< 0.05`.
- ORA sensitivity 분석의 통계 기준: raw p-value `< 0.05`.
- ORA 주 분석 입력: `padj < 0.05` 및 `|shrunken log2FC| > 1`.
- ORA sensitivity 입력: raw `pvalue < 0.05` 및 `|shrunken log2FC| > 1`.
- 주 분석과 sensitivity 분석에는 동일한 shrinkage 방법과 log2FC cutoff를 적용하여 통계 기준 변화만 비교한다.
- ORA의 p-value와 padj는 원 DESeq2 검정 결과를 사용하고, 효과크기 cutoff와 방향 판정에는 shrunken log2FC를 사용한다.
- 하나의 `A_vs_B` 결과표를 사용할 경우 양의 log2FC는 A 증가, 음의 log2FC는 B 증가로 분리한다.
- 방향별 contrast를 따로 만들 경우 항상 관심 방향이 양의 log2FC가 되도록 한다.
- GSEA rank는 유한한 값인 DESeq2 Wald statistic 전체를 사용한다.
- Count 생성에 사용된 annotation의 stable ID를 분석 기준으로 유지하고, gene symbol만 현재 공식 annotation으로 갱신한다.
- Count annotation과 GO·KEGG·Reactome pathway annotation의 출처·버전·조회일을 각각 기록한다.
- 분석 방법이 마크로젠 결과와 다르면 차이와 선택 근거를 명시한다.

## 7. Volcano plot

- 증가 유전자는 빨강 `#D62728`, 감소 유전자는 파랑 `#1F77B4`, 비유의 유전자는 연한 회색으로 표시한다.
- 제목과 부제목을 넣지 않는다.
- 사용자가 요청하지 않은 자동 marker label을 넣지 않는다.
- plot 상단 공간에 `Down regulation`과 `Up regulation`을 표시한다.
- 괄호를 사용하지 않는다.
- 첫 줄에는 상태, 둘째 줄에는 gene 수를 표시한다.
- Down 표기는 왼쪽 정렬, Up 표기는 오른쪽 정렬한다.
- gene 수 표시에 테두리를 넣지 않는다.
- legend가 있는 버전과 legend가 없는 대안 버전을 모두 유지한다.
- PPT 삽입을 이유로 그림 크기를 임의로 키우지 않는다. 기존 reference 또는 사용자가 지정한 크기를 따른다.

개별 gene label이 필요한지, 어떤 gene을 표시할지는 임의로 정하지 않고 사용자에게 묻는다.

## 8. ORA와 GSEA 그림

- ORA 점 색은 단색이 아니라 FDR 연속형 gradient를 사용한다.
- 낮은 FDR은 빨강, 높은 FDR은 파랑으로 표시한다.
- figure에 표시하는 pathway 제목은 첫 글자를 대문자로 표시한다.
- 원본 pathway ID와 결과표의 `Description` 값은 임의로 변경하지 않는다.
- p-value 또는 FDR이 `0.0001` 이하이면 그림에서는 `<0.0001`로 표시한다. 결과표의 숫자값은 변경하지 않는다.

### GSEA curve 고정 형식

- 기존 reference 코드를 그대로 사용한다.
- 그림 크기는 `5 × 3 inch`로 고정한다.
- PNG는 400 dpi, `2000 × 1200 px`로 저장한다.
- PDF도 `5 × 3 inch`로 저장한다.
- enrichment curve, gene-set hit/방향 띠, ranked-list metric의 3단 구조를 유지한다.
- x축 rank는 최하단 panel에만 표시한다.
- y=0 수평선을 넣지 않는다.
- y축의 0 눈금 및 `0.0` 표기를 제거한다.
- 기존 코드의 제목 줄바꿈, 색상, 글씨 크기와 HTML 표시 폭을 임의로 바꾸지 않는다.

## 9. 유전자 및 단백질 표기

- 사람 유전자 symbol은 모두 대문자 이탤릭으로 표기한다. 예: *TP53*, *IL6*.
- 생쥐와 랫드 유전자 symbol은 첫 글자만 대문자이고 나머지는 소문자 이탤릭으로 표기한다. 예: *Trp53*, *Il6*.
- 단백질명은 종과 관계없이 이탤릭을 사용하지 않는다. 예: TP53, IL-6.
- 다른 동물종은 해당 종의 공식 nomenclature를 확인하고 임의로 일반화하지 않는다.
- 변환 전 ID와 공식 symbol의 대응표를 보존하고 중복 symbol 처리 기준을 기록한다.

## 10. 통합 결과표

- GO, KEGG, Reactome 결과를 long format으로 통합한다.
- 한글 인코딩 문제를 피하기 위해 통합 파일은 Excel 형식으로 저장한다.
- 공통 열과 순서는 다음과 같다.

```text
Species, Contrast, Database, Ontology, Database_version,
ID, Description, Analysis, Direction, Effect_size_metric,
Effect_size, Input_gene_count, Overlap_count,
Background_overlap, Universe_size, p_value, FDR
```

- GSEA의 effect size는 NES, ORA의 effect size는 fold enrichment다.
- FDR은 `Contrast × Database × Ontology` 분석 family 안에서 계산·해석하고 서로 다른 family의 FDR을 전역 순위처럼 비교하지 않는다.
- `p_value`, `FDR`, NES와 fold enrichment는 Excel 숫자형으로 저장하고 문자열로 변환하지 않는다.
- Reactome 결과를 기존 Excel Table 밖에 붙이지 않고 GO·KEGG·Reactome 전체를 하나의 Excel Table 범위에 포함한다.
- Excel Table에 활성 정렬·필터 조건을 남기지 않는다.
- 기존 GO·KEGG 행과 값을 변경하지 않는다.
- 데이터를 불필요하게 wide format으로 만들지 않는다.

## 11. 단계별 지침

- `PROJECT_INFO.md`: 연구자가 확인한 프로젝트별 표본·design·contrast·annotation·분석 결정 사항
- `AGENTS_01_initial_analysis.md`: 마크로젠 method 정리와 전체 분석 설계
- `AGENTS_02_data_processing.md`: 입력 자료 점검, DESeq2와 contrast 구성
- `AGENTS_03_plot_visualization.md`: PCA, heatmap, DEG 및 enrichment 시각화
- `AGENTS_04_gsea.md`: KEGG, GO, Reactome GSEA
- `AGENTS_05_ora.md`: 방향별 KEGG, GO, Reactome ORA
- `AGENTS_06_network_plot.md`: 유전자–경로 및 경로–경로 네트워크

작업을 시작하기 전에 `PROJECT_INFO.md`와 해당 단계 문서를 직접 열어 읽고 공통 지침과 함께 적용한다.

모든 단계별 문서에는 루트 `AGENTS.md`를 우선 적용한다. 단계별 문서가 공통 지침과 충돌하면 임의로 선택하지 말고 공통 지침을 따르며 충돌 사실을 사용자에게 알린다.

## 12. 작업 속도와 소통

- 사용자가 표를 요청했는지 그림을 요청했는지 먼저 구분한다.
- “GSEA 저장”이 curve인지 결과표인지 애매하면 바로 질문한다.
- 중간에 문제를 발견하면 숨기지 말고 즉시 알린다.
- 최종 답변은 생성·수정한 파일, 개수, 크기, 검증 여부를 중심으로 간단히 보고한다.
- Git commit/push 여부를 명확히 말한다.
