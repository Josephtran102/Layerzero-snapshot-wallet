# Layerzero-snapshot-wallet
## I. Download và giải nén file `snapshot1_transactions.csv`:
[snapshot1_transactions.csv](https://layerzerodataset.s3.us-east-2.amazonaws.com/2024-05-15-snapshot1_transactions.csv.gz)

## II. Window cần cài đặt Git Bash:

[https://gitforwindows.org/](https://gitforwindows.org/)

[<img src='assets\Git.png' alt='banner' width= '100%'>]()

## II. Các câu lệnh Filter wallet: Sử dụng Terminal (Mac0S)/ Git Bash (Window)
### 1. Di chuyển vào thư mục chứa file snapshot:
```
cd /c/Users/Administrator/Desktop/Layerzero
```
[<img src='assets\cd.png' alt='banner' width= '100%'>]()

### 2. Filter tất cả các transaction của 1 địa chỉ ví sample:
```
grep -i "0x8964a0a2d814c0e6bf96a373f064a0af357bb4ce" snapshot1_transactions.csv
```
[<img src='assets\grep_all.png' alt='banner' width= '100%'>]()

### 3. Lọc địa chỉ ví theo tên Dapp đã tương tác:
- Tạo 1 file bash script với tên `wallet.sh` bằng lệnh:
```
nano wallet.sh
```
- Copy nội dung bên dưới paste vào file (Cần thay thế địa chỉ muốn filter).
[<img src='assets\bash_script.png' alt='banner' width= '100%'>]()
```
#!/bin/bash

wallet_address="0x8964a0a2d814c0e6bf96a373f064a0af357bb4ce"

# Lọc và đếm số lần tương tác với mỗi Dapp
interactions=$(grep "$wallet_address" snapshot1_transactions.csv | awk -F',' '{print $7}' | grep -v '^$' | sort | uniq -c | sort -nr)

# In ra chi tiết số lần tương tác với mỗi Dapp
echo "$interactions"

# Tính tổng số lần tương tác
total_interactions=$(echo "$interactions" | awk '{ sum += $1 } END { print sum }')

# In ra tổng số lần tương tác
echo "Total TXN: $total_interactions"
```
- Sau đó `Ctrl + O` --> `Enter` để Save file.
- Thoát ra trở về Terminal/Cmd: `Ctrl + X`
- Chạy lệnh bash để thực thi file `wallet.sh`
```
bash wallet.sh
```
- **Kết quả:*
[<img src='assets\Linux_result.png' alt='banner' width= '100%'>]()

[<img src='assets\win_rerult.png' alt='banner' width= '100%'>]()

### 4. Filter tất cả địa chỉ ví trong file snapshot và lưu vào file `addresses.txt`
**Ghi chú: Thời gian quét sẽ khá lâu, có tổng cộng 5,814,332 ví*
```
awk -F ',' '{print $5}' snapshot1_transactions.csv | sort -u > addresses.txt
```



