# nqucsie2024gcp
2024 gcp課程：筆記

本筆記嘗試使用[nb](https://xwmx.github.io/nb/#home)進行管理[^1]，commit 的標題若為`[nb]`開頭，代表該 commit 是透過`nb`操作的。
[^1]: I use Arch BTW.

## Topics:
- [http server](20241001.md#建立http伺服器)
- [Cloud Storage](20241008.md#upload-file-to-cloud-storage-and-copy-it-to-gce)
- [LAMP](20241015.md#database-server(lamp))
- [VPC Network](20241022.md#建立新的vpc-network)
- [Cloud SQL](20241029.md#cloud-sql)
- [Load Balancer](20241029.md#load-balancer)
- [期中練習](midterm.md)
- [Unmanaged load balancer block the direct access to VMs](20241112.md#block-the-direct-access-to-vm)
- [Managed load balancer](20241112.md#managed-load-balancer)
- [Cloud Router](20241119.md#cloud-router)
- [Monitoring & Alerting](20241119.md#monitoring-&-alerting)
- [伺服器Region與連線速度的關係](20241119.md#伺服器region與連線速度的關係)
- [使用Load Balancer透過HTTPS連線](20241119.md#使用load-balancer透過https連線)
- [Cloud CDN (Content Delivery Network)](20241119.md#cloud-cdn-(content-delivery-network))
- [建立沒有Externl IP的VM，用Managed Load Balancer連線](20241126.md#實驗)
- [進階負載均衡器](20241126.md#進階負載均衡器)
- [Internal Load Balancer & Cloud DNS](20241126.md#internal-load-balancer-&-cloud-dns)
- [Cloud Run Function](20241203.md#cloud-run-function)
- [Connecting to Cloud SQL with Cloud Functions using CLI (1)](20241203.md#connecting-to-cloud-sql-with-cloud-functions-using-cli)
- [Connecting to Cloud SQL with Cloud Functions using CLI（接續上週）](20241210.md#connecting-to-cloud-sql-with-cloud-functions-using-cli（接續上週）)
- [Google App Engine](20241210.md#google-app-engine)
- [Cloud RUN](20241210.md#cloud-run)
- [Cloud RUN（接續上週）](20241217.md#cloud-run)
- [建立 Artifact Registry](20241217.md#建立-artifact-registry)
- [Terraform](20241217.md#terraform)

## 常用
Http server auto creation script:
```bash
#!/bin/bash
apt update
apt -y install apache2
cat <<EOF > /var/www/html/index.html
<html><body><p>Linux startup script added directly. $(hostname -I)</p></body></html>
```

## 遇到的問題
在新的vpc建立load balancer時，需要reserve proxy-only subnet，有時會有奇怪bug，無法從網頁端reserve，必須直接從Cloud Shell用指令建立subnet<br>
參考：[Google Cloud 說明文件](https://cloud.google.com/load-balancing/docs/proxy-only-subnets#gcloud)<br>
範例：[proxy-only subnet](20241112.md#proxy-only-subnet)

## 遇到的問題 2
[第十五週](20241217.md#terraform-範例4) 的 Terraform 範例4，老師提供的 `variables.tf` 格式好像有問題，我有稍微做一點修正。

## 進度

| 週次（劃掉代表完成） | 連結 |
| :------------------: | :--: |
| ~~第一週~~ | [第一週 (2024/09/10)](20240910.md) |
| ~~第二週 (2024/09/17) （放假）~~ |
| ~~第三週~~ | [第三週 (2024/09/24)](20240924.md) |
| ~~第四週~~ | [第四週 (2024/10/01)](20241001.md) |
| ~~第五週~~ | [第五週 (2024/10/08)](20241008.md) |
| ~~第六週~~ | [第六週 (2024/10/15)](20241015.md) |
| ~~第七週~~ | [第七週 (2024/10/22)](20241022.md) |
| ~~第八週~~ | [第八週 (2024/10/29)](20241029.md) |
| ~~第九週~~ | [第九週 (2024/11/05) （期中）](midterm.md) |
| ~~第十週~~ | [第十週 (2024/11/12)](20241112.md) |
| ~~第十一週~~ | [第十一週 (2024/11/19)](20241119.md) |
| ~~第十二週~~ | [第十二週 (2024/11/26)](20241126.md) |
| ~~第十三週~~ | [第十三週 (2024/12/03)](20241203.md) |
| ~~第十四週~~ | [第十四週 (2024/12/10)](20241210.md) |
| ~~第十五週~~ | [第十五週 (2024/12/17)](20241217.md) |
