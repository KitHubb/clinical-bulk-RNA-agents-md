# 6. Network plot

## 목표

ORA/GSEA 결과의 pathway 간 중복과 핵심 유전자 연결을 시각화한다. 네트워크는 요약·탐색 도구이며 독립적인 통계 검정 결과로 취급하지 않는다.

## 기본 네트워크 유형

- gene–pathway network: 선택된 pathway와 기여 유전자 연결
- pathway–pathway network: 공유 유전자 또는 term similarity 기반 연결
- 필요 시 leading-edge network: GSEA pathway와 leading-edge gene 연결

## 입력 선정

- ORA 네트워크는 A 증가와 B 증가를 분리한다.
- GSEA 네트워크는 NES 부호를 보존한다.
- KEGG, GO, Reactome은 기본적으로 별도 network를 만들고, 통합 시 database 속성을 node에 표시한다.
- 시각화 대상 pathway는 유의성, 효과크기, 중복성을 고려해 사전에 정한 수로 제한한다.
- 화면 복잡도를 줄이기 위해 임의로 node/edge를 제거했다면 필터 기준을 기록한다.

## node와 edge 표현

- pathway node 크기: gene count 또는 set size 중 하나를 선택하고 legend에 명시
- pathway node 색: FDR 또는 NES
- gene node 색: log2FC
- edge: gene membership, shared gene count, Jaccard similarity 또는 semantic similarity
- edge threshold와 계산 방식을 기록한다.

색상 규칙은 전체 분석에서 통일한다. 예를 들어 A 증가/양의 NES는 따뜻한 색, B 증가/음의 NES는 차가운 색을 사용한다.

## label 규칙

- pathway label은 의미가 구분될 정도로 유지하고 과도하게 긴 경우 줄바꿈한다.
- gene symbol은 공식 nomenclature를 사용한다.
- 사람 유전자는 *TP53*처럼 모두 대문자 이탤릭, 생쥐/랫드는 *Trp53*처럼 첫 글자만 대문자 이탤릭을 원칙으로 한다.
- 이탤릭 적용이 그래프 도구에서 불안정하면 symbol 자체의 대소문자는 정확히 유지하고 caption에 규칙을 설명한다.

## 품질 관리

- isolated node 처리 방식을 명시한다.
- 허브가 단순히 큰 gene set 또는 annotation bias 때문에 생긴 것인지 확인한다.
- edge가 지나치게 많으면 threshold를 올리거나 pathway를 module별로 분리한다.
- graph layout seed를 고정하여 재현 가능하게 한다.
- 네트워크에 표시된 수와 전체 유의 결과 수를 caption에 함께 쓴다.

## 산출물

- pathway 및 gene node table
- edge table
- 사용한 threshold와 layout 설정
- 방향/데이터베이스별 network figure
- network에 포함된 pathway와 gene 목록
- 필요 시 Cytoscape 호환 파일

## 해석 원칙

- 연결이 곧 조절관계나 물리적 상호작용을 뜻하지 않는다.
- degree가 높은 gene을 자동으로 핵심 조절자로 단정하지 않는다.
- 네트워크 결과는 DEG, ORA/GSEA 통계, 문헌 및 실험 맥락과 함께 해석한다.
