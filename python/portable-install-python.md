## 安裝pip

1. 系統環境變數PATH要增加例如 `D:\Python;D:\Python\Scripts`
2. 下載pip: https://bootstrap.pypa.io/get-pip.py
3. 執行 `python get-pip.py`後會多出Lib & Scripts目錄
4. 編輯 python310._pth,增加 `Lib\site-packages`

## 升級pip

```bash=
python -m pip install --upgrade pip
```