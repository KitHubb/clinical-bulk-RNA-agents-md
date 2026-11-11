# 1. 초기 분석 및 method 정리

## 목표

마크로젠 기초 분석 파일에서 실제 적용된 방법을 먼저 추출하고, 추가 downstream 분석의 기준과 contrast를 확정한다. 이 단계에서는 보고서에 없는 방법을 마크로젠 방법인 것처럼 추정하지 않는다.

분석을 시작하기 전에 `PROJECT_INFO.md`를 연구자 답변과 실제 metadata로 작성한다. 비어 있거나 `unknown`인 필수 design 항목이 있으면 임의로 채우지 않고 질문한다.

## 마크로젠 기초 분석에서 확인할 항목

- reference genome과 annotation 버전
- read QC, adapter/quality trimming 도구와 기준
- aligner 또는 pseudo-aligner와 버전
- gene-level count 산출 도구 및 strandedness
- 제외된 샘플 또는 QC 기준
- DESeq2 전 row-sum 사전 필터 적용 여부
- 정규화 및 차등발현 분석 도구
- contrast 정의와 기준군(reference level)
- raw p-value와 FDR 보정 방법 및 DEG cutoff
- PCA, heatmap에 사용된 변환값과 유전자 선택 기준
- KEGG/GO ORA의 gene universe, ID 종류, cutoff, 데이터베이스 버전

확인 결과는 `confirmed`, `inferred`, `unknown`으로 구분한다. `unknown` 항목은 원 보고서 또는 실행 파일을 추가 확인할 대상으로 남긴다.

## 현재 가정하는 기초 분석 골격

- DESeq2 입력: row-sum 필터를 적용하지 않은 전체 raw integer count
- 차등발현 분석: DESeq2
- 탐색적 분석: PCA 및 heatmap
- 기존 downstream 분석: KEGG 및 GO 기반 ORA

`rowSums(counts) > 0`을 포함한 외부 사전 필터는 기본 분석에 적용하지 않는다. 마크로젠 코드에 별도 필터가 있으면 method 참고사항으로 기록하되, 현재 DESeq2 주 분석 입력과 혼합하지 않는다.

## 추가 분석의 초기 범위

- 데이터베이스: KEGG, Gene Ontology, Reactome
- 분석 유형: ORA 및 GSEA
- 1차 분석 contrast: 사용자가 확정한 전체 표본 pooled 주 비교군
- ORA:
  - 비교군에서 증가한 유전자 기반 결과 상위 15개
  - baseline에서 증가한 유전자 기반 결과 상위 15개
- GSEA: 전체 ranked gene list 기반으로 `Contrast × Database × Ontology`별 상위 10개

여기서 “상위”의 정렬 기준은 분석 전에 고정한다. 기본값은 FDR 오름차순이며, 동률이면 ORA는 gene ratio 또는 count, GSEA는 절대 NES를 보조 기준으로 사용한다. GSEA top 10은 데이터베이스와 ontology마다 별도로 선정하며, 양/음 결과가 한쪽에 치우치지 않는지 함께 확인한다.

## 초기 figure 목록

- 전체 표본 PCA
- 전체 발현 패턴 heatmap
- DESeq2 결과 기반 heatmap 1: `raw p < 0.05` 및 `|log2FC| > 1`
- DESeq2 결과 기반 heatmap 2: `FDR < 0.05` 및 `|log2FC| > 1`
- 방향별 ORA dot/bar plot: A 증가 top 15, B 증가 top 15
- GSEA 요약 plot: 데이터베이스·ontology별 top 10
- 대표 GSEA enrichment curve: 각 plot `5 × 3 inch`

## contrast 명세

시각화 또는 분석을 실행하기 전에 사용자에게 다음을 질문하고 확정 내용을 이 파일 또는 해당 프로젝트의 `AGENTS.md`에 기록한다.

- A와 B의 실제 임상/실험적 정의
- 포함 및 제외 표본
- 기준군과 비교군
- paired/repeated measure 여부
- batch, sex, age 등 공변량
- DESeq2 design formula
- 양의 log2FC가 의미하는 방향

## 완료 조건

- 마크로젠 method 요약표가 작성되어 있다.
- 확인되지 않은 방법이 명시되어 있다.
- sample metadata와 주 contrast가 확정되어 있다.
- 추가 분석 항목, cutoff, top N 선정 규칙이 기록되어 있다.
- 예정된 표와 figure 목록이 정리되어 있다.
