# 3. Plot 시각화

## 공통 규칙

- 모든 figure는 코드로 재생성 가능해야 한다.
- 작업 전에 비교군과 baseline을 사용자에게 확인한다.
- 기존 reference 코드나 그림이 있으면 크기·색·폰트·배치·파일 형식을 직접 확인하고 그대로 따른다.
- 사용자가 요청하지 않은 디자인 변경과 marker label을 추가하지 않는다.
- 제목보다 축, 단위, 비교 방향, 표본 수, cutoff가 명확해야 한다.
- 색상은 group 간 일관되게 유지하고 색각이상 친화적 palette를 사용한다.
- 최종 저장 시 width, height, unit, dpi를 코드에 명시한다.
- raster figure는 기본 300 dpi 이상, 선과 글자가 중요한 figure는 PDF/SVG 같은 vector 형식도 함께 저장한다.
- figure caption에 사용 데이터, 변환, scaling, 거리, clustering, cutoff를 간결하게 기록한다.

## PCA

- DESeq2 VST 또는 rlog 값을 사용한다.
- 기본적으로 전체 DESeq2 입력 유전자 또는 사용자가 승인한 high-variable gene 기준을 사용한다.
- PC1과 PC2 축에 설명 분산 비율을 표시한다.
- 색은 주 group, 모양은 필요 시 batch/sex/paired ID 등 두 번째 변수를 표현한다.
- sample label은 겹침을 최소화하되 outlier 식별이 가능해야 한다.
- outlier를 그림만 보고 임의 제거하지 않는다.

## 전체 heatmap

- “total heatmap”의 유전자 선택 기준을 반드시 명시한다.
- 모든 유전자를 그대로 표시해 해석 불가능하면 상위 변동 유전자 사용을 허용하되 개수와 선정 방법을 기록한다.
- VST/rlog matrix를 기본으로 하고, gene별 패턴 비교 시 row z-score를 사용할 수 있다.
- 거리 함수와 linkage method를 기록한다.
- annotation에는 최소한 group을 포함하고 필요하면 batch/sex 등 공변량을 추가한다.
- 열 순서를 임의 고정했는지 clustering했는지 명시한다.

## DEG heatmap

두 결과를 별도로 생성한다.

1. `pvalue < 0.05` 및 `|log2FC| > 1`
2. `padj < 0.05` 및 `|log2FC| > 1`

- 첫 번째를 “유의한 DEG”로 단정하지 말고 raw p-value 기준임을 제목과 caption에 쓴다.
- 두 번째는 BH-FDR 기준임을 명시한다.
- 유전자 수가 너무 많으면 선정 규칙을 사전에 고정하고 전체 목록은 별도 표로 제공한다.
- heatmap cell에는 VST/rlog 값 또는 row z-score를 사용하며 raw count를 직접 색상값으로 사용하지 않는다.

## 유전자 발현 plot

- FPKM, TPM, CPM, normalized count 중 실제 사용 단위를 y축과 caption에 쓴다.
- 사람 유전자: *TP53*처럼 모두 대문자 이탤릭.
- 생쥐/랫드 유전자: *Trp53*처럼 첫 글자만 대문자 이탤릭.
- 단백질명은 이탤릭을 사용하지 않는다.
- 개별 표본 점을 가능하면 표시하고, error bar가 SD/SEM/CI 중 무엇인지 명시한다.
- 작은 표본에서 bar plot만 제시하지 않는다.

## Volcano plot

- 증가 유전자는 `#D62728`, 감소 유전자는 `#1F77B4`, 비유의 유전자는 연한 회색으로 표시한다.
- 제목과 부제목을 넣지 않고 자동 marker label을 추가하지 않는다.
- 상단에는 괄호 없이 첫 줄 `Down regulation`/`Up regulation`, 둘째 줄 gene 수를 표시한다.
- Down은 왼쪽, Up은 오른쪽 정렬하고 gene 수 표시에 테두리를 넣지 않는다.
- legend가 있는 버전과 없는 버전을 모두 유지한다.
- 그림 크기는 reference 또는 사용자 지정값을 따르고 PPT용이라는 이유로 임의 확대하지 않는다.
- 개별 gene label의 대상은 사용자에게 확인한다.

## enrichment figure

- ORA는 방향(A 증가/B 증가)과 데이터베이스를 구분한다.
- ORA 점 색은 낮은 FDR이 빨강, 높은 FDR이 파랑인 연속형 gradient로 표시한다.
- GSEA는 NES의 부호와 contrast 방향을 일관되게 표시한다.
- dot size, color, x축이 각각 Count, GeneRatio, NES, FDR 중 무엇을 뜻하는지 legend에 쓴다.
- 지나치게 긴 pathway 이름은 의미가 손실되지 않는 범위에서 줄바꿈한다.
- pathway 표시 제목은 첫 글자를 대문자로 하되 원본 ID와 결과표 `Description`은 바꾸지 않는다.
- 그림에서 p-value 또는 FDR이 0.0001 이하이면 `<0.0001`로 표시하되 결과표 숫자는 유지한다.
- GSEA curve는 reference 코드의 3단 구조, 색상, 글씨, 줄바꿈을 유지한다.
- GSEA curve는 PNG `5 × 3 inch`, 400 dpi, `2000 × 1200 px`와 PDF `5 × 3 inch`로 저장한다.
- GSEA curve의 x축 rank는 최하단 panel에만 두고 y=0 수평선과 y축 0/`0.0` 눈금을 제거한다.

## 파일명 예시

- `PCA_all_samples.pdf`
- `Heatmap_total_VST_top_variable.pdf`
- `Heatmap_DEG_pvalue0.05_absLFC1.pdf`
- `Heatmap_DEG_FDR0.05_absLFC1.pdf`
- `ORA_KEGG_A_up_top15.pdf`
- `GSEA_Reactome_top10.pdf`

파일명에 공백과 모호한 `final`, `new`, `test` 표기를 피한다.
