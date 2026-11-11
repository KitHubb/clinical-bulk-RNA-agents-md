# 5. ORA

## 목표

주 비교 `A vs B`에서 방향별 DEG를 분리하여 KEGG, GO, Reactome over-representation analysis를 수행한다.

## 입력 유전자 집합

ORA는 주 분석과 마크로젠 기준 sensitivity 분석을 모두 수행한다.

- 주 분석 통계 기준: `padj < 0.05`
- sensitivity 통계 기준: raw `pvalue < 0.05`
- 주 분석 입력: `padj < 0.05` 및 `|shrunken log2FC| > 1`
- sensitivity 입력: raw `pvalue < 0.05` 및 `|shrunken log2FC| > 1`
- 주 분석과 sensitivity 분석에는 동일한 shrinkage 방법과 log2FC cutoff를 적용하여 통계 기준 변화만 비교한다.
- p-value와 padj는 원 DESeq2 검정 결과를 사용하고 효과크기 cutoff와 방향 판정에는 shrunken log2FC를 사용한다.
- 단일 `A_vs_B` 결과에서 A 증가는 shrunken log2FC `> 1`, B 증가는 shrunken log2FC `< -1`로 분리한다.

방향별 contrast를 따로 생성하면 관심 방향이 양수가 되도록 설정하고 shrunken log2FC `> 1`을 적용한다. 주 분석과 sensitivity 결과는 `Analysis` 열과 파일명에서 명확히 구분한다.

## gene universe

- 주 분석 universe는 해당 contrast에서 `padj`가 NA가 아니며 pathway ID로 유효하게 매핑된 전체 유전자다.
- sensitivity universe는 해당 contrast에서 raw `pvalue`가 NA가 아니며 pathway ID로 유효하게 매핑된 전체 유전자다.
- genome 전체나 데이터베이스 전체 유전자를 무조건 background로 사용하지 않는다.
- A 증가와 B 증가 분석에서 동일한 원칙의 universe를 사용한다.
- 입력/배경의 매핑 전후 유전자 수, 중복 제거 수와 실제 universe 크기를 기록한다.

## 분석 구성

각 데이터베이스를 방향별로 별도 실행한다.

- KEGG: A 증가 / B 증가
- GO BP: A 증가 / B 증가
- Reactome: A 증가 / B 증가

필요 시 GO CC/MF를 추가하되 BP와 섞지 않는다.

## 통계 및 결과 선정

- 과대표현 검정과 다중검정 보정 방법을 명시한다.
- 기본 유의 기준은 BH-FDR `< 0.05`이다.
- 방향별·데이터베이스별 top 15를 제시한다.
- 기본 정렬은 FDR 오름차순이며, 동률이면 GeneRatio 또는 Count 내림차순을 사용한다.
- 유의 term이 15개 미만이면 있는 것만 표시한다.
- 유의 term이 없으면 결과 없음으로 보고하고 cutoff를 사후 변경하지 않는다.

## 결과표 필수 열

- database 및 ontology
- pathway/term ID
- pathway/term description
- GeneRatio
- background ratio
- gene count
- raw p-value
- adjusted p-value/FDR
- 입력 gene ID 및 symbol
- enrichment 방향(A 증가 또는 B 증가)

GO, KEGG, Reactome의 최종 통합본은 Excel long format으로 저장하고 다음 공통 열을 사용한다.

```text
Species, Contrast, Database, Ontology, Database_version,
ID, Description, Analysis, Direction, Effect_size_metric,
Effect_size, Input_gene_count, Overlap_count,
Background_overlap, Universe_size, p_value, FDR
```

ORA의 `Effect_size_metric`은 fold enrichment로 하고 `(Overlap_count / Input_gene_count) / (Background_overlap / Universe_size)`로 계산한다. `Effect_size`, counts, `p_value`, `FDR`은 Excel 숫자형을 유지한다. Reactome을 포함한 모든 행을 하나의 Excel Table 범위에 넣고 활성 정렬·필터는 제거한다. 기존 GO·KEGG 행과 값 및 원본 `Description`은 변경하지 않는다.

FDR은 `Contrast × Database × Ontology × Analysis` family 안에서 계산·해석한다. 주 분석과 sensitivity 분석, 서로 다른 database/ontology의 FDR을 전역 순위처럼 비교하지 않는다.

## 시각화

- A 증가와 B 증가를 별도 panel 또는 명확히 구분된 facet으로 표시한다.
- 각 방향 top 15를 기본으로 한다.
- dot plot에서는 x축, 점 크기, 색의 의미를 legend에 쓴다.
- pathway 이름이 잘리지 않도록 줄바꿈과 여백을 조정한다.
- 데이터베이스 간 GeneRatio를 직접 비교할 때 gene universe와 ontology 차이를 고려한다.

## 해석 원칙

- ORA의 “증가/감소”는 pathway 자체의 활성 방향이 아니라 해당 방향 DEG 목록의 과대표현을 의미한다.
- 서로 겹치는 유전자 때문에 유사 pathway가 반복될 수 있음을 확인한다.
- 소수 유전자에 의해 형성된 결과는 count와 gene 목록을 함께 검토한다.
