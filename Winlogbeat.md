# Winlogbeat

1. [Overview](#overview)

2. [Deployment](#deployment)

3. [Research](#research)

<a name="overview"></a>

## 1. Overview

Winlogbeat giúp đẩy windows event logs về Elasticsearch hoặc Logstash, có thể cài đặt chúng như một Windows service trên Windows XP hoặc nhưng phiên bản sau đó.

Winlogbeat sẽ đọc các event logs sử dụng Windows APIs, lọc các events theo cấu hình của người dùng, sau đó gửi event data đến Elasticsearch hoặc Logstash. Winlogbeat luôn theo dõi event log, đảm bảo khi có event log mới chúng đuợc gửi đi một cách kịp thời. Vị trí đọc cho mỗi event log được lưu lại, cho phép Winlogbeat tiếp tục sau khi restarts.

Winlogbeat có thể bắt được event data từ bất kỳ một event logs nào đang chạy trên hệ thông, như :
- application events
- hardware events
- security events
- system events 	

<a name="deployment"></a>
## 2. Deployment
### Requirements
	windows server 2008 R2
	Hệ thống ELK đã cài beat, cấu hình nhận trên các cổng sau:
	- Elasticsearch: 192.168.169.223:9200
	- Logstash: 192.168.169.220:5044
	 

### Installtion

[Step 1: Installing Winlogbeat](#step1)

[Step 2: Configuring Winlogbeat](#step2)

[Step 3: Configuring Winlogbeat to Use Logstash](#step3)

[Step 4: Starting Winlogbeat](#step4)

[Step 5: Loading Sample Kibana Dashboards](#step5)


<a name="step1"></a>
### Step 1: Installing Winlogbeat 

* Download Winlogbeat tại [Đây]("https://www.elastic.co/downloads/beats/winlogbeat")
* Giải nén ra `C:\Program Files` và đổi tên thư mục thành `Winlogbeat`
* Mở PowerShell dưới quyền Administrator
* Chạy các lệnh sau trên PowerShell để install 
		
		PowerShell.exe -ExecutionPolicy UnRestricted -File .\install-service-winlogbeat.ps1
		cd 'C:\Program Files\Winlogbeat'
		.\install-service-winlogbeat.ps1


<a name="step2"></a>
### Step 2: Configuring Winlogbeat

Sửa file winlogbeat để configure

	1. Thêm các event logs muốn theo dõi
		
	winlogbeat.event_logs:
	  - name: Application
	  - name: Security
	  - name: System

Chạy `Get-EventLog *` trên PowerShell để xem danh sách các event logs có sẵn

	2. Nếu muốn output elasticsearch, set địa chỉ IP của elasticsearch:

	output.elasticsearch:
	  hosts:
	    - localhost:9200


<a name="step3"></a>
### Step 3: Configuring Winlogbeat to Use Logstash

Nếu muốn output logstash bỏ `#` đầu các dòng dưới đây (IP của logstash):

	output.logstash:
	  hosts: ["127.0.0.1:5044"]

Test configuration file, chạy lệnh: `.\winlogbeat.exe -c .\winlogbeat.yml -configtest -e`

Ví dụ file configure output Elasticsearch [winlogbeat.yml](winlogbeat.yml)

<a name="step4"></a>
### Step 4: Starting Winlogbeat

Start Winlogbeat service với lệnh sau:

	PS C:\Program Files\Winlogbeat> Start-Service winlogbeat

Stop Winlogbeat
	
	PS C:\Program Files\Winlogbeat> Stop-Service winlogbeat

<a name="step5"></a>
### Step 5: Loading Sample Kibana Dashboards

#### Importing the Dashboards

Vào thư mục cài đặt winlogbeat chạy

	PS > scripts\import_dashboards.exe

Mở Kibana lên, trên Discover đã có `winlogbeat-*	`

<img src = "img\kibana">

#### Report
<img src="img\1.png">

<img src="img\2.png">

<img src="img\3.png">

<img src="img\4.png">

<img src="img\5.png">

<a name="research"></a>
## 3. Research
Tham khảo: https://www.elastic.co/guide/en/beats/winlogbeat/current/winlogbeat-getting-started.html
 
