# REFACTORING

* 모듈 호출 방식 변경
  * `python translate.py` 로 파일을 직접 실행하는 방식을 `python -m src...` 처럼 src 패키지 내부 모듈을 호출하는 방식으로 변경
* 설정 및 로깅 중앙화 (`src.config`, `src.core.logger`)
  * 각 단계 (`Proc`, `Core`, `Merge`)가 시작될 때 `src.config`를 통해 설정을 로드하고 검증
  * `src.utils`에서 LRU Cache 사용하여 용어집(Glossary) 로딩 속도 최적화
* 디렉토리 구조 변경
  * 결과물 저장 위치를 `./output/pot`, `./output/po`, `./resources/glossary` 등으로 체계화
* 병렬 처리 로직 변경
  * `translate.py`에서 `ThreadPoolExecutor`를 사용하도록 병렬 처리 로직 변경

# STRUCTURE

* `src.config`
  * 프로그램이 무엇을 가지고 어떻게 돌아갈지 설정
  * 값이 올바른지 검사하고, 프로그램 전체에 뿌려줌
  * EX. 설정 파일 로드(`load_config()`), 유효성 검증(`validate_config()`), 특정 설정값 조회(`get_config_value()`) 등
* `src.utils`
  * 반복적으로 사용되는 단순 작업 수행
  * 설정값에 의존하기보다, 인자를 받아서 결과를 뱉는 함수 위주 (범용성, 재사용성)
  * EX. JSON 파일을 열어서 읽음(`load_json()`), URL을 받아 파일 다운로드(`download_file()`), 로그 세팅 함수 (`setup_logger()`) 등
