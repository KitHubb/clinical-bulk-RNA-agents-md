# 4. GSEA

## 목표

모든 검정 가능 유전자를 순위화하여 KEGG, GO, Reactome gene set의 일관된 상향·하향 변화를 평가한다. DEG cutoff로 유전자를 먼저 잘라낸 목록을 GSEA 입력으로 사용하지 않는다.

## 데이터베이스

- KEGG
- Gene Ontology: BP를 기본으로 하며, CC/MF 사용 시 결과를 분리한다.
- Reactome

종(species), gene set 출처, 다운로드 또는 실행 날짜, 데이터베이스/패키지 버전을 기록한다.

- Count annotation stable ID를 기준으로 gene set ID를 매핑하고 symbol은 표시용으로만 최신화한다.
- Reactome은 선택한 release의 versioned mapping을 재사용한다. release 번호, 다운로드 URL 또는 API/database version, retrieval date, 파일 checksum과 ReactomePA/관련 패키지 버전을 기록한다.
- live Reactome 결과를 사용할 때는 실행 시 database version을 조회해 기록하며, 재실행 시 버전이 바뀌면 동일 분석으로 간주하지 않는다.

## ranked gene list

- 주 비교의 비교군과 baseline은 실행 전에 사용자에게 확인한다.
- 양의 rank와 양의 NES가 비교군에서 높은 방향, 음의 값이 baseline에서 높은 방향이 되도록 통일한다.
- rank metric은 DESeq2 Wald statistic으로 고정한다.
- 일부 DEG만 사용하지 않고 `is.finite(stat)`인 전체 유전자를 사용한다.
- Wald statistic을 다른 조합 metric으로 임의 대체하지 않는다. statistic이 없거나 사용할 수 없으면 사용자에게 알리고 중단한다.
- NA/Inf는 제외하고 개수를 보고한다.
- 동일 gene ID가 여러 번 있으면 대표값 선택 기준을 명시한다.
- rank는 내림차순 정렬하고 유전자 수, 최솟값, 최댓값을 점검한다.

## 분석 파라미터

- gene set 최소/최대 크기를 명시한다.
- permutation 또는 multilevel 알고리즘, seed, score type을 기록한다.
- 다중검정은 BH-FDR을 기본으로 한다.
- 데이터베이스별로 별도 분석하고 임의로 p-value를 합치지 않는다.

## 결과 선정

- 기본 요약은 `Contrast × Database × Ontology`별 top 10 pathway이다.
- 기본 정렬은 FDR 오름차순, 동률이면 `|NES|` 내림차순이다.
- FDR은 각 `Contrast × Database × Ontology` family 안에서 계산하고 서로 다른 family의 FDR을 전역 순위로 비교하지 않는다.
- 양/음 방향 결과를 각각 보존하고, family별 top 10에서 한 방향만 과도하게 차지하는지 확인한다.
- 유의 pathway가 10개 미만이면 존재하는 결과만 표시하고 top 10을 채우기 위해 cutoff를 완화하지 않는다.
- 유의 결과가 없으면 “없음”으로 보고하고 nominal p-value 결과를 확정적 결과처럼 대체하지 않는다.

## 산출물

- 전체 pathway 결과표
- database·ontology별 top 10 요약표 및 dot/bar plot
- 양의 NES와 음의 NES 분리 표
- 주요 pathway leading-edge gene 표
- 대표 pathway enrichment curve

대표 GSEA curve는 기존 reference 코드를 사용하여 다음 형식으로 저장한다.

- PNG: `5 × 3 inch`, 400 dpi, `2000 × 1200 px`
- PDF: `5 × 3 inch`
- enrichment curve, gene-set hit/방향 띠, ranked-list metric의 3단 구조
- x축 rank는 최하단 panel에만 표시
- y=0 수평선과 y축 0/`0.0` 눈금 제거
- 기존 제목 줄바꿈, 색상, 글씨 크기, HTML 표시 폭 유지

curve에는 다음 통계를 표시한다.

- pathway 이름
- NES
- nominal p-value
- FDR
- A/B 방향

## 해석 원칙

- NES 부호는 contrast와 함께 해석한다.
- 비슷한 GO term이 반복될 경우 중복성을 설명하거나 별도 단순화 결과를 제공한다.
- leading-edge gene을 개별 DEG와 동일시하지 않는다.
- GSEA 결과는 pathway 활성의 간접 근거이며 기능적 활성의 직접 측정으로 단정하지 않는다.
