- `grep -r "import flaky"` , `grep -r "import flaky"` 를 사용하면 현재 디렉토리 위치에서 재귀적으로 하부 디렉토리 안까지 `import flaky`가 포함되어 있는 파일이 어디에 있는지 출력한다.
```bash


❯ grep -r "from flaky"
./agent_api/tests/test_assessment.py:from flaky import flaky

```

```plain text

현재디렉토리/
├── file1.py
├── folder1/
│   ├── file2.py
│   └── subfolder/
│       └── file3.py
└── folder2/
    └── file4.py

```
