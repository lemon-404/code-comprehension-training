# 훈련 기록 폴더 구조

훈련 한 건을 `training/<한글문제명>/` 폴더 하나에 담습니다.

```text
code-comprehension-training/
├── FOLDER_STRUCTURE.md
├── README.md
├── TEMPLATE.md
├── TEMPLATE_EXAMPLE.md
└── training/
    └── <한글문제명>/
        ├── 풀이과정.md
        ├── example/
        └── solution/
```

- `example/`에는 읽고 분석할 코드를 둡니다. 분석 대상이 여러 Java 파일로 구성되어 있다면 원래 파일명과 패키지 구조를 유지해 함께 둡니다.
- `solution/`에는 직접 작성한 풀이를 둡니다. `training/`을 기준으로 패키지는 `<한글문제명>.solution`을 사용합니다. 한글 문제명에서 공백을 빼고, Java 파일명과 클래스명에는 문제 영문명을 사용합니다. 예를 들어 기사단원의 무기 풀이는 `기사단원의무기/solution/KnightsWeapon.java`에 둡니다.
- `풀이과정.md`에는 [TEMPLATE.md](TEMPLATE.md) 양식에 따라 분석과 회고를 기록합니다. 분석 대상 항목에는 원본 출처와 읽은 파일 또는 메서드를 적습니다. 재현 코드가 있다면 실제 경로를 적습니다. 작성 예시는 [TEMPLATE_EXAMPLE.md](TEMPLATE_EXAMPLE.md)를 참고합니다.
