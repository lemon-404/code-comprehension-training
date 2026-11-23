# 훈련 기록 폴더 구조

훈련 한 건을 `training/<날짜>-<주제>/` 폴더 하나에 담습니다.

```text
code-comprehension-training/
├── FOLDER_STRUCTURE.md
├── README.md
├── TEMPLATE.md
├── TEMPLATE_EXAMPLE.md
└── training/
    └── <날짜>-<주제>/
        ├── analysis.md
        ├── example/
        └── solution/
```

- `example/`에는 읽고 분석할 코드를 둡니다. 분석 대상이 여러 Java 파일로 구성되어 있다면 원래 파일명과 패키지 구조를 유지해 함께 둡니다.
- `solution/`에는 원본을 가리고 직접 재현한 코드를 둡니다.
- `analysis.md`에는 [TEMPLATE.md](TEMPLATE.md) 양식에 따라 분석과 회고를 기록합니다. 분석 대상 항목에는 원본 출처와 읽은 파일 또는 메서드를 적습니다. 재현 코드가 있다면 실제 경로를 적습니다. 작성 예시는 [TEMPLATE_EXAMPLE.md](TEMPLATE_EXAMPLE.md)를 참고합니다.
