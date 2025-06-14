# 지하철역 내부 상가 최적 입지 분석 프로젝트

서울교통공사의 지하상가 임대정보와 유동인구 데이터를 활용하여 클러스터링 기법을 통해 지하철역별 상권 특성을 분석하고 최적의 상가 입지를 추천하는 머신러닝 프로젝트를 구현하였습니다.  
K-Means, DBSCAN, Mean Shift 등 다양한 클러스터링 알고리즘을 적용하여 해당 역의 상권을 특성을 분석하고 최적의 입지를 추천합니다.

---

## 코드 실행 환경

- **Python**: 3.10.16  
- **scikit-learn (sklearn)**: 1.6.1  
- **seaborn**: 0.13.2  
- **기타 주요 라이브러리**:
  - pandas
  - numpy
  - matplotlib
 
## 사용 방법

- `서울교통공사_일별통행통계_20241231.csv`, `서울교통공사_지하상가 임대정보_2024MMDD.csv`, `main.ipynb` 을 같은 경로에 두고 실행
---

## 파일 구조 및 설명

### `main.ipynb`

- 전체 실험 흐름을 포함한 Python 기반 Jupyter Notebook입니다.
- 데이터 로딩 → 전처리 → 클러스터링 분석 및 비교 → 특징 분석 → 결과 해석 순서로 구성되어 있습니다.

---

### Data/

- `서울교통공사_지하상가 임대정보_2024MMDD.csv`  
  > 역명, 상가번호, 업종, 면적, 임대료 등 포함된 월별 임대 데이터입니다. (10개 파일)
  
- `서울교통공사_일별통행통계_20241231.csv`  
  > 연령별(미성년, 성인, 노인, 외국인), 시간대별 월별 유동인구 데이터입니다. (용량이 커서 따로 제출하겠습니다)

---

## Result/ 폴더 구조

### Result/Preprocessing/

> 데이터 전처리 및 통합 파일 저장

- `proprecessed_2401.csv` ~ `proprecessed_2412.csv`: 월별 임대정보 원본 데이터를 전처리한 결과입니다.
- `population_2401.csv` ~ `population_2412.csv`: 월별 유동인구 원본 데이터를 전처리한 결과입니다.
- `Preprocessed_pop_2401.csv` ~ `Preprocessed_pop_2412.csv`: population_24XX.csv 파일을 preprocessing_population() 함수로 2차 전처리한 최종 결과입니다. 
- `Rental_month_sum.csv`, `population_month_sum.csv`, `Rental_sumby_month.csv`: 월별 통합 데이터셋입니다.
- `merged.csv`: 월별 통합 데이터셋들을 합친 최종 통합 데이터셋입니다.

---

### Result/Model/

> 클러스터링 알고리즘 실행 결과 저장

 K-Means 결과:

- `kmeans(total_population).csv`: 총 유동인구를 기준으로 한 K-Means 클러스터링 결과입니다.
- `kmeans(adult_population).csv`: 성인 유동인구 패턴 기반 클러스터링 결과입니다.
- `kmeans(minor_population).csv`: 미성년 유동인구 패턴 기반 클러스터링 결과입니다.
- `kmeans(old_population).csv`: 노인 유동인구 패턴 기반 클러스터링 결과입니다.
- `kmeans(foreign_population).csv`: 외국인 유동인구 패턴 기반 클러스터링 결과입니다.
- `kmeans(density).csv`: 상가 밀도 기반 클러스터링 결과입니다.
- `kmeans(potential).csv`: 상권 잠재력 지수 기반 클러스터링 결과입니다.

 DBSCAN 결과:

- `dbscan(adult_population).csv` ~ `dbscan(potential).csv`: DBSCAN 알고리즘을 사용한 밀도 기반 클러스터링 결과입니다. (5개 파일)

 Mean Shift 결과:

- `meanshift(total_population).csv` ~ `meanshift(potential).csv`: Mean Shift 알고리즘을 사용한 클러스터링 결과입니다. (7개 파일)

---

### Result/Characteristic/

> 클러스터별 특성 분석 결과

- `total_population_characteristic.csv`: 총 유동인구 기준 클러스터별 특성 분석 결과입니다. 각 클러스터의 평균 유동인구, 주요 업종, 임대료 수준 등의 특성이 정리되어 있습니다.
- `adult_population_characteristic.csv`: 인 유동인구 패턴에 따른 클러스터 특성 분석 결과입니다.
- `minor_population_characteristic.csv`: 미성년 유동인구 패턴에 따른 클러스터 특성 분석 결과입니다.
- `old_population_characteristic.csv`: 노인 유동인구 패턴에 따른 클러스터 특성 분석 결과입니다.
- `foreign_population_characteristic.csv`: 외국인 유동인구 패턴에 따른 클러스터 특성 분석 결과입니다.
- `density_characteristic.csv`: 상가 밀도 기준 클러스터별 특성 분석 결과입니다.
- `potential_characteristic.csv`: 상권 잠재력 지수 기준 클러스터별 특성 분석 결과입니다.


---

### Result/Suggestion/

> 최종 입지 추천 결과 저장

- `result_df.csv`: 클러스터링 결과와 역별 특성이 결합된 최종 분석 데이터입니다. 각 지하철역의 클러스터 소속 정보, 유동인구 특성, 상권 유형, 추천 업종 등이 종합적으로 정리되어 있습니다.
- `location_suggestion.csv`: 입지 추천 시스템의 최종 결과 파일입니다. 각 지하철역별로 업종별 수, 상권 특성, 입지 추천 등 실용적인 정보를 제공합니다.
