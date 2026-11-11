# 프로젝트별 분석 정보

이 문서는 프로젝트마다 실제 연구자에게 확인한 내용과 실제 metadata·파일에서 검증한 사실을 기록한다. 추정으로 채우지 않는다. 각 항목은 `confirmed`, `inferred`, `unknown` 중 하나로 표시하고, `inferred`와 `unknown`은 분석 시작 전에 연구자에게 확인한다.

## 1. 프로젝트 개요

- 프로젝트명:
- 연구 목적 및 1차 가설:
- 종(species):
- 조직/세포 유형:
- 분석 단위:
- 연구자 확인일:
- 확인 담당자:
- 상태:

## 2. 입력 자료와 count provenance

- Raw count 파일:
- Sample metadata 파일:
- Count 생성 도구 및 버전:
- Alignment/quantification 방식:
- Reference genome 및 build:
- Count 생성 GTF/annotation 출처와 release:
- Strandedness:
- Feature level(gene/transcript):
- Stable ID 유형 및 version suffix 처리:
- featureCounts/HTSeq integer count 또는 Salmon/tximport estimated count 여부:
- 실제 파일 확인 근거:
- 상태:

## 3. Biological unit와 replicate

- Biological unit(patient/animal/tissue/library 등):
- Biological replicate 정의:
- Technical replicate 존재 여부:
- Technical replicate가 동일 library의 반복 sequencing인지:
- 조건별 독립 biological replicate 수:
- Biological replicate가 없는 비교 존재 여부:
- 상태:

## 4. Paired/repeated structure

- Paired 여부:
- Repeated-measure 여부:
- Patient/subject ID 열:
- Time point:
- Subject별 조건 조합 완전성:
- 누락된 pair 또는 time point:
- 상태:

## 5. 비교군과 contrast

| Contrast | 비교군/numerator | Baseline/denominator | 양의 log2FC 의미 | 포함 표본 | 제외 표본 | 상태 |
|---|---|---|---|---|---|---|
|  |  |  |  |  |  |  |

- 전체 pooled 비교의 임상적 의미:
- 층화 또는 interaction 검토가 필요한 subgroup:
- Primary contrast:
- Secondary/exploratory contrast:

## 6. Batch 및 임상 공변량

검토 대상: sequencing batch, library batch, 기관, RIN, sex, age, medication, stage, tissue composition 및 프로젝트 관련 임상 변수.

| 변수 | design 포함 여부 | 포함/제외 근거 | 결측 수 | group과 confounding 여부 | 상태 |
|---|---:|---|---:|---|---|
|  |  |  |  |  |  |

- `table(group, batch)` 등 교차표 확인 결과:
- 완전 confounding 존재 여부:
- 완전 confounding 시 연구자 결정:

## 7. 포함·제외 및 outlier 기준

- 분석 전 정의된 포함 기준:
- 분석 전 정의된 제외 기준:
- QC failure 기준:
- Outlier 판정 기준:
- PCA만으로 표본을 제외하지 않았는지:
- 제외 표본, 사유 및 승인자:
- 상태:

## 8. DESeq2 design 검증

- Design formula:
- Reference level:
- Interaction term:
- Model matrix dimension:
- Model matrix rank:
- Full rank 여부:
- 표본 수:
- Coefficient 수:
- `resultsNames(dds)`:
- Wald test 사용 여부:
- 검증 결과 및 남은 문제:
- 상태:

## 9. DEG 및 ORA 기준

- 주 DEG FDR cutoff: `0.05`
- DESeq2 `results(alpha=...)`: `alpha=0.05`
- DEG 표시 log2FC cutoff: `|log2FC| > 1`
- ORA 주 분석: `padj < 0.05` 및 `|shrunken log2FC| > 1`
- ORA sensitivity: raw `pvalue < 0.05` 및 `|shrunken log2FC| > 1`
- ORA log2FC cutoff 적용 여부: 적용
- ORA log2FC cutoff 값: `|shrunken log2FC| > 1`
- LFC shrinkage 방법 및 package version:
- LFC shrinkage coefficient:
- ORA 방향 분리 방식:
- ORA 결과 표시: database·ontology·방향별 top 15
- 주 분석과 sensitivity에 동일한 log2FC cutoff 적용 여부: 적용
- 결정 근거 및 연구자 승인:
- 상태:

## 10. GSEA 기준

- Rank metric: finite DESeq2 Wald statistic 전체
- Database/ontology별 minGSSize:
- Database/ontology별 maxGSSize:
- Algorithm 및 주요 parameter:
- Random seed:
- 결과 표시: `Contrast × Database × Ontology`별 top 10
- 양/음 NES 표시 규칙:
- 상태:

## 11. Annotation 및 pathway database

- 분석 기준 stable ID:
- Symbol 갱신 annotation 출처 및 release:
- Stable ID → 최신 symbol 매핑 성공/실패/중복 수:
- GO release 또는 annotation package version:
- KEGG source 및 조회일:
- Reactome release 번호:
- Reactome versioned mapping 위치/URL:
- Reactome retrieval date:
- Reactome mapping checksum:
- ReactomePA 및 관련 package version:
- Count annotation과 pathway annotation 차이:
- 상태:

## 12. ORA universe

- 주 분석 universe 정의: `padj` non-NA 및 pathway ID 매핑 가능 유전자
- Sensitivity universe 정의: raw `pvalue` non-NA 및 pathway ID 매핑 가능 유전자
- Contrast별 universe 크기:
- ID 매핑 전후 universe 크기:
- 중복 ID 처리 방식:
- 상태:

## 13. 저장 및 재현 정보

- 결과 폴더 `result/날짜_분석명/`:
- `dds_input` RDS:
- `dds_fitted` RDS:
- 재현 스크립트:
- R version:
- Package/session information 파일:
- 기존 결과 덮어쓰기 승인 여부:
- Git commit/push 요청 여부:

## 14. Figure reference와 출력 결정

- Reference 코드/그림 위치:
- Figure 크기·폰트·색·배치 확인 결과:
- Volcano gene label 대상:
- Legend 포함/미포함 버전:
- GSEA curve 출력 형식:
- 기타 사용자 지정 사항:

## 15. 미확정 사항과 연구자 질문

| 우선순위 | 질문 | 결과에 미치는 영향 | 답변 | 상태 |
|---|---|---|---|---|
|  |  |  |  |  |

필수 항목이 `unknown`인 상태에서는 관련 분석을 임의 설정으로 진행하지 않는다.
