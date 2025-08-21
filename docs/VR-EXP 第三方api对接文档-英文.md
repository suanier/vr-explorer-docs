**Contents**

```plain
1. API overview and test data 
2. Interface naming rules 
3. Interface description 
	3.1 Description of the third-party customer contact 
		3.1.1 Acquisition of the third-party interface credential
		3.1.2 Interface for acquiring QR code from the third party
		3.1.3 Push of the synthesis notification by Visbody 
		3.1.4 The third-party status code 
	3.2 Acquisition of Visbody interface credential
		3.2.1 Acquisition of Visbody interface credential
		3.2.2 Binding of user information
	3.3 Acquisition of anthropometry file and data
		3.3.1 Acquisition of body composition data
			3.3.1-1 vr-explorer Acquisition of body composition data
    	3.3.1-2 S30 Acquisition of body composition data
		3.3.2 Acquisition of user fat grade
		3.3.3 Acquisition of body score
		3.3.4 Acquisition of body composition adjustment data
	3.4 Acquisition of posture file and data
		3.4.1 Acquisition of posture file
		3.4.2 Acquisition of posture data
		3.4.3 Acquisition of circumference file
		3.4.4 Acquisition of circumference data
	3.5 Acquisition of shoulder test data
	3.6 Acquisition of report printing
	3.7 Description of Visbody return status code
	3.8 Description of relationship between the synthesis push type and interface
```



## 1. File description
### 1.1 API overview
| Interface  | Interface function  |
| :--- | --- |
| Interface of acquiring the third-party credential  | Acquire the address where the third-party interface credential can be accessed. It shall be provided by the third-party customer, refer to 3.1.1 for interface format and input return parameters  |
| Request of the third-party QR code interface  | The third-party customer is responsible for providing the QR code of the result after the user completes the measurement at the device end, refer to 3.1.2 for interface format and input return parameters.  |
| Push of the synthesis notification by Visbody  | Call the interface to push the anthropometry, posture, segment distribution results and scan-related information to the third-party services. It shall be provided by the third-party customer, refer to 3.1.3 for interface format and input return parameters  |
| Acquisition of Visbody interface credential  | The third-party customer calls this interface to obtain Visbody-related interface credentials, vfid and vfsecret can also be obtained from the Visbody management platform  |
| Bind user information  | When scanning the QR code of the device through the third-party app, the third party app will call this interface to inform the Visbody background service to initiate the synthesis request after the user's identity is verified by the background of the third-party app.  |
| Data interface of anthropometry file  | After the anthropometry model is synthesized successfully, the third-party customer will call this interface to get the anthropometry model and data.  |
| Data interface of posture file  | After the posture model is synthesized successfully, the third-party customer will call this interface to get the posture model and data.  |
| Description of return status code  | Description of status code  |


### 1.2 Test data
| Parameter name  | Parameter value  |
| :--- | :--- |
| key | <font style="color:rgb(23, 26, 29);">vfbuqo5Tmnvrqe25</font> |
| secret | <font style="color:rgb(23, 26, 29);">GpBIk52k83PGvyzkeFDtfCUs3kaX8vUg</font> |
| scanid | <font style="color:rgb(23, 26, 29);">35042104080001-7254507f-50da-11ec-b365-300ed55248eb</font> |


## 2. Interface rules
### 2.1 Naming rules
`https: //[domain name]/[version No.]/[interface name] `

For example: [<u><font style="color:#0000FF;">http://api.explorer.visbody.com/v1/token</font></u>](http://api.vr-explorer.visbody.com/v1/token)

| Example  | Description  |
| :--- | :--- |
| api.explorer.visbody.com | Domain name  |
| v1 | Version No.  |
| token | Interface name  |


### 2.2 Version control
Interface version is controlled by routing 

```plain
HTTP GET:
// v1 
http://api.explorer.visbody.com/v1/token
// v2 
http://api.explorer.visbody.com/v2/token
```

### 2.3 POST submission methods
`Content-Type: application/json`

## 3. Interface description
### 3.1 Description of the third-party customer contact
`Currently support the API connection and QR code connection. `

#### 1. API connection
**Operation instruction:**

User measurement data can be acquired through the API interface provided by Visbody. 

After successful connection, Visbody will push the scan ID and relevant information through the 3.1.3 interface ∂ configured by the customer, the customer can obtain the data after accessing the corresponding interface according to the results of measurement items, see 3.8 for the relationship between the synthesis push type and interface 

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21651137/1631523418195-c0f80ab8-a9d1-4992-8624-accd7f249f9b.jpeg)



**Connection description:**

+ `Apply for API connection privilege` 
+ `Configure 3.1.1 and 3.1.3 interfaces through API connection settings in the Visbody management platform `

#### 2. QR code connection
**Operation instruction:**

Replace the default serial number of the device, after scanning the code, jump to the customer's own APP or other platforms such as mini program, which requires customers to develop their own business logic for code scanning. 

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21651137/1631523439940-828bcb72-4887-42fb-b84f-8c2dec5dbffe.jpeg)



**Connection description:**

+ Apply for API connection privilege 
+ Configure 3.1.1, 3.1.2 and 3.1.3 interfaces through API connection settings in the Visbody management platform 
+ After scanning the QR code, Visbody 3.2.2 interface will initiate the user information binding and synthesis through the VPS 3.2.2 interface 

### 3.1.1 Acquisition of the third-party interface credential :id=third-token
**Interface description:**

The customer should provide the address of the credentials to access third-party interfaces such as 3.1.2 and 3.1.3. 

**Request URL format requirements:**

+ `<http|https>: //<domain name>/<path> `
+ `http://<host>:<port>/<path>`

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| key | Yes  | string | Unique user credential, provided by the third party  |
| secret | Yes  | string | Unique user credential secret key, provided by the third party  |


**Return example under normal operation**

```plain
  {
    "code": 0,
    "data": {
      "token": "TOKEN",
      "expires_in": 7200
    }
  }
```

**Return example under operation error**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 30001,
    "error_msg": 'ERROR_MSG'
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| code | int | Status code, refer to 3.1.5 for return status code  |
| error_msg | string | Error information  |
| token | string | Interface credential  |
| expires_in | int | Credential valid time (s)  |


### 3.1.2 Interface for acquiring QR code from the third party :id=get-grcode
**Interface description:**

QR code of results after the user has completed the measurement at the device end, provided by the third party 

**Request URL format requirements:**

    - `<http|https>: //<domain name>/<path> `
    - `http://<host>:<port>/<path>`

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| scan_id | Yes  | string | Scan ID  |
| device_id | Yes  | string | Device ID  |
| token | Yes  | string | Third-party interface credential  |


**Return example under normal operation**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
      "url": "qrCodeUrl",
    }
  }
```

**Return example under operation error**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 30001,
    "error_msg": 'ERROR_MSG'
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| code | int | Status code, refer to 3.1.4 for return status code  |
| error_msg | string | Error information  |
| url | string | QR code address  |


### 3.1.3 Push of the synthesis notification by Visbody :id=notify
**Interface description:**

After scanning the QR code of the device, call the interface to push the anthropometry, posture, segment distribution results and scan-related information to the third-party services. 

**Request URL format requirements:**

+ `<http|https>: //<domain name>/<path> `
+ `http://<host>:<port>/<path>`

**Request method:**

+ POST

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| scan_id | Yes  | string | Scan ID  |
| device_id | Yes  | string | Device ID  |
| time | Yes  | date | Completion time, time format, 2018-06-25 10:10:10  |
| user_info | No  | object | User information  |
| age | No  | int | Age  |
| birthday | No  | string | Date of birth  |
| phone | No  | string | Mobile phone No.  |
| sex | No  | string | f for female, m for male  |
| height | No  | int | Height  |
| action_status | Yes  | object | Status information  |
| girth_status | No  | int | Circumference synthesis state, 0 for failure, 1 for success, 2 for timeout  |
| eval_status | No  | int | Posture synthesis state, 0 for failure, 1 for success, 2 for timeout  |
| bia_status | No  | int | Body composition synthesis state, 0 for failure, 1 for success, 2 for timeout  |
| eval_shoulder_status | No  | int  | Shoulder synthesis state, 0 for failure, 1 for success, 2 for timeout  |
| pdf_status  | No  | int  | Whether the pdf can be printed, 0 for no, 1 for yes  |
| token  | Yes  | string  | Third-party interface credential  |


**The format is as follows:**

```plain
{
  "data": {
    "age": 26,
    "birthday": "1992-12-12",
    "height": 178,
    "phone": "13812345678",
    "sex": "f"
  },
  "action_status":{
    "eval_status": 0,
    "bia_status": 0,
    "girth_status": 0,
    "eval_shoulder_status": 0
  },
  "device_id": "35041910080001",
  "scan_id": "35041910080001-a210136e-1bfb-11ea-b711-00d861a9ecd9",
  "time": "2019-12-11 17:56:15",
  "token": "TOKEN"
}
```

**Return example under normal operation**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0
  }
```

Notification requests must be responded to when received correctly, otherwise the Visbody service will initiate 3 retransmissions 

**Return example under operation error**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 30001,
    "error_msg": 'ERROR_MSG'
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| code | int | Status code, refer to 3.1.4 for return status code  |
| error_msg | string | Error information  |


### 3.1.4 The third-party status code
Description: As for the above interface, the third-party customer should return the relevant status information according to the following status code 

| Status code  | Remark  |
| :--- | :--- |
| 0 | Request succeeded  |
| 30001 | Invalid token  |


## 3.2 Acquisition of Visbody interface credential :id=get-token
#### Request header has parameters

+ Return the report data in the corresponding language and units according to the parameters in the request header 

Add languages and units to the request header 

```plain
$headers[]  =  "Unit: $vfUnit; // vfUnitTransferable parameter units are metric (metric unit) / imperial (imperial unit) 
$headers[]  =Transferable languages are en-US(English) / ja-JP(Japanese) / zh-CN(Chinese) "Language: $vfLanguage; // vfLanguage
```

### 3.2.1 Acquisition of Visbody interface credential
**Interface description:**

+ Obtain the credentials for calling the Visbody interface 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/token](http://api.vr-explorer.visbody.com/v1/token)

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| key | Yes  | string | Unique credential of the third-party user, namely, vfid  |
| secret | Yes  | string | Unique credential secret key of the third-party user, namely, vfsecret  |


**Return example under normal operation**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
      "token": "TOKEN",
      "expires_in": 7200
    }
  }
```

**Return example under operation error**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 40001,
    "error_msg": 'ERROR_MSG'
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| code | int | Status code, refer to 3.8 for return status code  |
| error_msg | string | Error information  |
| token | string | Interface credential  |
| expires_in | int | Credential valid time (s)  |


**Acquisition methods of vfid and vfsecret:**

1. Create an account on the Visbody management system 

2. Apply for granting the API connection privilege 

3. The vfid and vfsecret can be obtained after the device connection page of the Visbody management system completes the API connection. 

**token use method**

1. Input as a normal parameter in a request via POST or GET 

2. Add Authorization Bearer Token to the request header, taking php as an example 

```plain
$headers[]  =  "Content-Type: application/json";
$headers[]  =  "Authorization: Bearer ". $vfToken;
```

### 3.2.2 Binding of user information
**Interface description:**

+  When scanning the QR code of the device through the third-party app, the third party app will call this interface to inform the Visbody background service to initiate the synthesis request after the identities of user and equipment is verified by the background of the third-party app. 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/dataBind](http://api.vr-explorer.visbody.com/v1/dataBind)

**Request method:**

+ POST

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| --- | --- | :--- | --- |
| scan_id | Yes  | string | Scan ID  |
| device_id | Yes  | string | Device ID  |
| third_uid | Yes  | string | Unique ID of the third-party user, with the letters and numbers of total 8-40 characters  |
| mobile | Yes  | string | Mobile phone No.  |
| sex | Yes  | int | Gender 1. Male 2. Female  |
| height | Yes  | int | Height (cm) 110-205  |
| age | Yes  | int | note that the age range should be between 10 and 99  |
| token | Yes  | string | Interface credential  |
| name | yes | string | What about users |


**Return example under normal operation**

```plain
  {
    "code": 0,
    "data": {
      "result":TRUE
    }
  }
```

**Return example under operation error**

```plain
 {
    "code": 40001,
    "error_msg": 'ERROR_MSG'
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| code | int | Status code, refer to 3.6 for return status code  |
| error_msg | string | Error information  |
| result | boolean | Initiate synthesis results  |


**token use method**

1. Input as a normal parameter in a request via POST or GET 

2. Add Authorization Bearer Token to the request header, taking php as an example 

```plain
$headers[]  =  "Content-Type: application/json";
$headers[]  =  "Authorization: Bearer ". $vfToken;
```

## 3.3 Acquisition of anthropometry file and data
### 3.3.1 Acquisition of body composition data
#### 3.3.1-1 vr-explorer Acquisition of body composition data
**Interface description:**

+ Obtain the anthropometry and body composition data 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/body/mass](http://api.vr-explorer.visbody.com/v1/body/mass)

**Request method:**

+  GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| token | Yes  | string | Interface credential  |
| scan_id | Yes  | string | Scan ID  |


**Return example**

```plain
  {
    "code": 0,
    "data": {
    	"WT": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "FFM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "BFM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "LM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "TBW": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "BMI": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "PBF": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "BMR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "WHR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "SM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "TM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		  "PROTEIN": {"l":10,"m":15,"h":20,"v":30.3,"status":3}，
    }
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| WT | object | Weight (kg)  |
| FFM | object | Fat free weight (kg)  |
| BFM | object | Body fat mass (kg)  |
| LM | object | Muscle mass (kgt)  |
| TBW | object | Body water content (kg)  |
| BMI | object | Body mass  |
| PBF | object | Body fat percentage (%)  |
| BMR | object | Basal metabolic mass (kcal/d)  |
| WHR | object | Waist-hip ratio  |
| SM | object | Skeletal muscle mass (kg)  |
| TM | object | Inorganic salt (kg)  |
| PROTEIN | object | Protein (kg)  |


**Description of body composition range**

```plain
{
	"l":10,        // Lower-limit value 
	"m":15,        // Standard value 
	"h":20,        // Upper-limit value 
	"v":30.3,      // Measured value 
	"status":3     // Status: 1 for low, 2 for normal, 3 for high 
}
```



#### 3.3.1-2 S30 vr-explorer Acquisition of body composition data
**Interface description:**

+ Obtain the anthropometry and body composition data 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/body/mass](http://api.vr-explorer.visbody.com/v1/body/mass)

**Request method:**

+  GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| --- | --- | --- | :--- |
| token | Yes  | string | Interface credential  |
| scan_id | Yes  | string | Scan ID  |
| type | No | string | Device type:  1 is S30 data type |


**Return example**

```plain
  {
    "code": 0,
    "data": {
    	"WT": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"FFM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BFM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"TBW": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BMI": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"PBF": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BMR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"WHR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"SM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"TM": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"PROTEIN": {"l":10,"m":15,"h":20,"v":30.3,"status":3}，
		"ICW": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"ECW": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
    "METABOLICAGE": {
            "l": 0,
            "m": 0,
            "h": 0,
            "v": 22,
            "status": 0
        },
    }
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | --- |
| WT | object | Weight (kg)  |
| FFM | object | Fat free weight (kg)  |
| BFM | object | Body fat mass (kg)  |
| LM | object | Muscle mass (kgt)  |
| TBW | object | Body water content (kg)  |
| BMI | object | Body mass  |
| PBF | object | Body fat percentage (%)  |
| BMR | object | Basal metabolic mass (kcal/d)  |
| WHR | object | Waist-hip ratio  |
| SM | object | Skeletal muscle mass (kg)  |
| TM | object | Inorganic salt (kg)  |
| PROTEIN | object | Protein (kg)  |
| ICW | object | Intracellular Water（kg） |
| ECW | object | Extracellular Water（kg） |
| METABOLICAGE | object | METABOLICAGE |


**Description of body composition range**

```plain
{
	"l":10,        // Lower-limit value 
	"m":15,        // Standard value 
	"h":20,        // Upper-limit value 
	"v":30.3,      // Measured value 
	"status":3     // Status: 1 for low, 2 for normal, 3 for high 
}
```

### 3.3.2 Acquisition of user fat grade
**Interface description:**

+  User fat grade 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/body/state](http://api.vr-explorer.visbody.com/v1/body/state)

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| token | Yes  | string | Interface credential  |
| scan_id | Yes  | string | Scan ID  |


**Return example**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
			"va_grade":9,
			"body_age":18,
			"body_share":3
    }
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| va_grade | int | Visceral fat grade (1-10 for normal, 10-14 for excessive high, 14-17 for high, >17 super high)  |
| body_age | int | Physical age  |
| body_share | int | Somatotype (1 for weak, 2 for muscular, 3 for obese, 4 for healthy)  |


### 3.3.3 Acquisition of user body score
**Interface description:**

+ User body score 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/body/score](http://api.vr-explorer.visbody.com/v1/body/score)

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| token | Yes  | string | Interface credential  |
| scan_id | Yes  | string | Scan ID  |
| scan_type | Yes  | int | Scan type: 1 for anthropometry, 2 for posture  |


**Return example**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
      "score": 98
    }
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| score | int | Item scoring  |


### 3.3.4 Acquisition of body composition adjustment data
**Interface description:**

+ Obtain the body composition adjustment data 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/forecast/adjust](http://api.explorer.visbody.com/v1/forecast/adjust)

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| token | Yes  | string | Interface credential  |
| scan_id | Yes  | string | Scan ID  |


**Return example**

```plain
  {
    "code": 0,
    "data": {
      "weight":1.8,
	  	"body_fat":-2.6,
	  	"muscle":-2.6,
	  	"gr_weight":75,
	  	"gr_body_fat":12.3,
	  	"gr_muscle":15.1,
    }
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| weight | double | Weight regulating variable  |
| body_fat | double | Body fat mass regulating variable  |
| muscle | double | Muscle regulating variable  |
| gr_weight | double | Weight golden ratio  |
| gr_body_fat | double | Body fat mass golden ratio  |
| gr_muscle | double | Muscle golden ratio  |




### 3.3.5 <font style="color:rgba(0, 0, 0, 0.85);">Obtain Segmental Data</font>
**<font style="color:rgba(0, 0, 0, 0.85);">Interface Description:</font>**

+ **<font style="color:rgba(0, 0, 0, 0.85);">Used to obtain segmental data</font>**

**<font style="color:rgba(0, 0, 0, 0.85);">Request URL:</font>**

+ `**http://api.explorer.visbody.com**/v1/seg/spread/data`

**<font style="color:rgba(0, 0, 0, 0.85);">Request Method:</font>**

+ GET

**<font style="color:rgba(0, 0, 0, 0.85);">Parameters:</font>**

| **Parameter Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| token | Yes | string | Interface credential |
| scan_id | Yes | string | Scan ID |


**<font style="color:rgba(0, 0, 0, 0.85);">Return Example:</font>**

```plain
  {
    "code": 0,
    "data": {
    	"BFMLA": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BFMRA": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BFMTR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BFMLL": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"BFMRL": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LMLA": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LMRA": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LMTR": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LMLL": {"l":10,"m":15,"h":20,"v":30.3,"status":3},
		"LMRL": {"l":10,"m":15,"h":20,"v":30.3,"status":3}
    }
  }
```

**<font style="color:rgba(0, 0, 0, 0.85);">Description of Return Parameters:</font>**

| Parameter Name | Type | Description |
| --- | --- | --- |
| BFMLA | object | Left upper limb segmental fat (kg) |
| BFMRA | object | Right upper limb segmental fat (kg) |
| BFMTR | object | Trunk segmental fat (kg) |
| BFMLL | object | Left lower limb segmental fat (kg) |
| BFMRL | object | Right lower limb segmental fat (kg) |
| LMLA | object | Left upper limb segmental muscle (kg) |
| LMRA | object | Right upper limb segmental muscle (kg) |
| LMTR | object | Trunk segmental muscle (kg) |
| LMLL | object | Left lower limb segmental muscle (kg) |
| LMRL | object | Right lower limb segmental muscle (kg) |


*Description of segment range**

```plain
  {
"l":10, // Lower limit value
"m":15, // Standard value
"h":20, // Upper limit value
"v":30.3, // Measured value
"status":3 // Status: 1: Low, 2: Normal, 3: High
}
```

**<font style="color:rgba(0, 0, 0, 0.85);">Description of Range Parameters:</font>**

| Parameter Name | Type | Description |
| --- | --- | --- |
| l | double | Lower limit value |
| m | double | Standard value |
| h | double | Upper limit value |
| v | double | Measured value |
| status | int | Status: 1: Low, 2: Normal, 3: High |




## 3.4 Acquisition of posture file and data
### 3.4.1 Acquisition of posture file
**Interface description:**

+ Obtain the posture model, key point json file and posture photo 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/shape/file](http://api.vr-explorer.visbody.com/v1/shape/file)

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| token | Yes  | string | Interface credential  |
| scan_id | Yes  | string | Scan ID  |


**Return example**

<font style="color:#595959;">  </font>

```plain
{
    "code": 0,
    "data": {
      "model_url": "MODEL_URL",
	  	"json_url": "JSON_URL",
	  	"pic_front_url": "PIC_FRONT_URL",
	  	"pic_left_url": "PIC_LEFT_URL",
	  	"pic_right_url": "PIC_RIGHT_URL",
	  	"pic_top_url": "PIC_TOP_URL",
      "expires_in": 7200
    }
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | --- |
| model_url | string | Posture model file path, with file format of .obj  |
| json_url | string | Posture key point json file, with the file format of .txt (for displaying the body model effect key point)  |
| pic_front_url | string | File path of posture front view, with the file format of .jpg  |
| pic_left_url | string | File path of posture left view, with the file format of .jpg  |
| pic_right_url | string | File path of posture right view, with the file format of .jpg  |
| pic_top_url | string | File path of posture top view, with the file format of .jpg  |
| pic_back_url | string | Path to the image file for the back view of the posture, file format. jpg |
| expires_in | int | Valid time of file path  |


### 3.4.2 Acquisition of posture data
**Interface description:**

+ Obtain the posture evaluation data 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/shape/points](http://api.vr-explorer.visbody.com/v1/shape/points)

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| token | Yes  | string | Interface credential  |
| scan_id | Yes  | string | Scan ID  |


**Return example**

```plain
  {
    "code": 0,
    "data": {
		"high_low_shoudler": {
			"val": 15.96,
			"conclusion": "Uneven shoulders (higher in the left)" 
			"risk": "Uneven shoulders will trigger chronic pain in the neck and shoulders, often accompanied by scoliosis, pelvic displacement, and leg length discrepancy" 
		},
		"head_slant": {
			"val": 15.96,
			"conclusion": "Normal", 
			"risk": "--"
		},
		"head_forward": {
			"val": 15.96,
			"conclusion": "Normal", 
			"risk": "--"
		},
		"leg_xo": {
			"left_val": 184.3,
			"right_val": 187.2,
			"conclusion": "Normal", 
			"risk": "--"
		},
		"pelvis_forward": {
			"val": 15.96,
			"conclusion": "Normal", 
			"risk": "--"
		},
		"left_knee_check": {
			"val": 175.4,
			"conclusion":"Normal", 
			"risk": "--"
		},
		"right_knee_check": {
			"val": 187.7,
			"conclusion": "Normal", 
			"risk": "--"
		},
		"round_shoulder_left": {
			"val": 15.6,
			"conclusion":"Normal", 
			"risk": "--"
		},
		"round_shoulder_right": {
			"val": 15.6,
			"conclusion":"Normal", 
			"risk": "--"
		},
		"body_slope": {
			"val": 0,
			"conclusion":"Normal", 
			"risk": "--"
		}
    }
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| high_low_shoudler | object | Uneven shoulders (cm/in)  |
| head_slant | object | Head lateroversion, unit  |
| head_forward | objectze | Head anteversion, unit  |
| leg_xo | object | Leg type, unit  |
| pelvis_forward | object | Pelvic displacement, unit  |
| left_knee_check | object | Left knee analysis, unit  |
| right_knee_check | object | Right knee analysis, unit  |
| round_shoulder_left | object | Left round shoulder, unit  |
| round_shoulder_right | object | Right round shoulder, unit  |
| body_slope | object | Body lean, unit  |
| val | double | Measured value  |
| left_val | double | Measured value of left leg  |
| right_val | double | Measured value of right leg  |
| conclusion | string | Evaluation conclusions  |
| risk | string | Risk warning, "--" for normal  |


### 3.4.3 Acquisition of circumference file
**Interface description:**

+ Obtain the body circumference model, circumference json, model circumference picture files 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/measure/file](http://api.vr-explorer.visbody.com/v1/measure/file)

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| token | Yes  | string | Interface credential  |
| scan_id | Yes  | string | Scan ID  |


**Return example**

```plain
  {
    "code": 0,
    "data": {
      "model_url": "MODEL_URL",
      "json_url": "JSON_URL",
	    "pic_measure_url": "PIC_MEASURE_URL",
      "expires_in": 7200
    }
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| model_url | string | Anthropometry model file path, with file format of .obj  |
| json_url | string | Circumference json file path, with the file format of .json, can be used with the model to display the position and data of corresponding circumference  |
| pic_measure_url | string | File path of model circumference, with the file format of .jpg  |
| expires_in | int | Valid time of file path  |


### 3.4.4 Acquisition of circumference data
**Interface description:**

+ Obtain the circumference data after anthropometry 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/measure/girth](http://api.vr-explorer.visbody.com/v1/measure/girth)

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| token | Yes  | string | Interface credential  |
| scan_id | Yes  | string | Scan ID  |


**Return example**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": {
     "bust_girth": 0,
      "waist_girth": 0,
      "hip_girth": 0,
      "left_upperArm_girth": 0,
      "right_upperArm_girth": 0,
      "left_thigh_girth": 0,
      "right_thigh_girth": 0,
      "left_calf_girth": 0,
      "right_calf_girth": 0,
      "height": 0,
      "neck_girth": 0,
      "left_min_thigh_girth": 0,
      "right_min_thigh_girth": 0,
      "mid_waist_girth": 0
    }
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| --- | :--- | --- |
| neck_girth | double | <font style="color:rgb(38, 38, 38);">Neck circumference</font>(cm/in) |
| left_upper_arm_girth | double | <font style="color:rgb(38, 38, 38);">Left upper arm</font>(cm/in) |
| right_upper_arm_girth | double | <font style="color:rgb(38, 38, 38);">Right upper arm</font>(cm/in) |
| bust_girth | double | <font style="color:rgb(38, 38, 38);">Bust</font>(cm/in) |
| waist_girth | double | <font style="color:rgb(38, 38, 38);">High Waist</font>(cm/in) |
| mid_waist_girth | double | <font style="color:rgb(38, 38, 38);">Mid Waist</font>(cm/in) |
| hip_girth | double | <font style="color:rgb(38, 38, 38);">Hipline</font>(cm/in) |
| left_thigh_girth | double | <font style="color:rgb(38, 38, 38);">Left thigh</font>(cm/in) |
| left_min_thigh_girth | double | <font style="color:rgb(38, 38, 38);">Minimum circumference of left thigh</font>(cm/in) |
| right_thigh_girth | double | <font style="color:rgb(38, 38, 38);">Right thigh circumference</font>(cm/in) |
| right_min_thigh_girth | double | <font style="color:rgb(38, 38, 38);">Minimum circumference of right thigh</font>(cm/in) |
| left_calf_girth | double | <font style="color:rgb(38, 38, 38);">Left calf circumference</font>(cm/in) |
| right_calf_girth | double | <font style="color:rgb(38, 38, 38);">Right calf circumference</font>(cm/in) |
| height | double | Enter height(cm/in) |


## 3.5 Obtain the shoulder test data and conclusion
**Interface description:**

+ Obtain the shoulder test data and conclusion 

**Request URL:**

+ [http://api.explorer.visbody.com/v1/shoulder/data](http://api.vr-explorer.visbody.com/v1/shoulder/data)

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| token | Yes  | string | Interface credential  |
| scan_id | Yes  | string | Scan ID  |


**Return example**

```plain
  {
    "code": 0,
    "data": {
		"left_abuction": {
			"val": 25.5,
			"conclusion": "Limited", 
			"limit": "[150.0 ~180.0 ]"
		},
		"right_abuction": {
			"val": 25.5,
			"conclusion":"Limited", 
			"limit": "[150.0 ~180.0 ]"
		},
		"left_antexion": {
			"val": 45.5,
			"conclusion":"Excessive", 
			"limit": "[120.0 ~180.0 ]"
		},
		"right_antexion": {
			"val": "--",
			"conclusion": "--",
			"limit": "--"
		},
		"conclusions": [
			{
				"title":"Limited motion of the shoulder joint", 
				"analysis":"The limited motion of the shoulder joint is mostly caused by muscular tension, insufficient range of motion of clavicle and scapula, and neck scapula not in the neutral position. It will interfere with normal motion mode (causing athletic injury), causes related pathological problems (such as scapulohumeral periarthritis, hunchback, cervical spine pain), and may lead to various shoulder joint diseases if it is neglected for a long period. ", 
				"advice":"Please invite professionals to further seek for concrete reasons. " 
			},
			{
				"title":"Excessive motion of the shoulder joint", 
				"analysis":"The excessive motion of the shoulder joint is mostly caused by the slack ligament (mostly seen in females). For example, the frequent shoulder flexibility training may also lead to excessive activity. ", 
				"advice": "Please invite professionals to further seek for concrete reasons. " 
			}
		]
		
    }
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| left_abuction | object | Abduction and upthrow - left hand  |
| right_abuction | object | Abduction and upthrow - right hand  |
| left_antexion | object | Anteflexion and upthrow - left hand  |
| right_antexion | object | Anteflexion and upthrow - right hand  |
| val | double | Measured value, unit: ( ), -- for failure  |
| limit | string | Normal range, -- for failure  |
| conclusion | string | Evaluation conclusions, -- for failure  |
| conclusions | array | All conclusions of this test, there may be a single conclusion or multiple conclusions based on the test condition.  |
| title | string | Conclusion title  |
| analysis | string | Conclusion analysis. If all items are normal, return the null character string  |
| advice | string | Conclusion suggestion. If all items are normal, return the null character string  |


## 3.6 Acquisition of report printing
**Interface description:**

+ Obtain the file interface of the report printing

**Request URL:**

+ [http://api.explorer.visbody.com/v1/report](http://api.vr-explorer.visbody.com/v1/reprot)

**Request method:**

+ GET

**Parameter:**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | :--- |
| token | Yes  | string | Interface credential  |
| scan_id | Yes  | string | Scan ID  |


Request header has parameters 

Language of the report 

$headers[]  =  "Language: $vfLanguage; // vfLanguageTransferable languages are en-US(English) / ja-JP(Japanese) / zh-CN(Chinese) 

**Return example**

<font style="color:#595959;"> </font>

```plain
 {
    "code": 0,
    "data": "http://rexp-dev.visbody.com/reptfile/report/vr-exp/pdf/en-US/35042104086001-d0e23a52-0095-11ec-9e7e-300ed55249b1.pdf?_upt=9baa05fa1629365732.003"
  }
```

**Return parameter description**

| Parameter name  | Type  | Description  |
| :--- | :--- | :--- |
| data | string | File download link  |


## 3.7 Description of Visbody return status code
The interface response of Visbody is distinguished by HTTP status code and business status code, and the business status code is marked in the response body 

| HTTP status code  | Business status code  | Remark  |
| :--- | :--- | :--- |
| 500 | -1 | The system is busy  |
| 200 | 0 | Request succeeded  |
| 400 | 40001 | Invalid request path  |
| 400 | 40002 | Invalid vfid or secret  |
| 400 | 40003 | Invalid token  |
| 400 | 40004 | Parameter error  |
| 400 | 40005 | Failure of binding device information  |
| 400 | 40006 | Invalid scan ID (scan ID does not belong to the account or this scan is not successful or the scan data are expired)  |
| 400 | 40008 | No interface privilege  |
| 400 | 40009 | Scan ID is bound  |


## 3.8 Description of relationship between the synthesis push type and interface 
**Description:**

+ After the third-party customer has configured the 3.1.3 interface for API connection, Visbody will push the relevant measured data to the third party through this interface for each measurement by the user, and the third party will access the corresponding Visbody data result interface according to the synthetic result status of the measuring items 

**Relationship between the measuring items and the synthesis push type is as follows:**

| Measuring items  | Synthesis push item  |
| :--- | :--- |
| Body circumference  | girth_status |
| Posture  | eval_status |
| Body composition  | bia_status |
| Shoulder  | eval_shoulder_status |
| Report printing  | pdf_status |


**Relationship between the synthesis push type and interface is as follows:**

| Synthesis push item  | Description  | Accessible interface  |
| :--- | :--- | :--- |
| girth_status | Body circumference  | 3.4.3,3.4.4 |
| eval_status | Posture  | 3.3.3,3.4.1,3.4.2 |
| bia_status | Body composition  | 3.3.1,3.3.2,3.3.3,3.3.4 |
| eval_shoulder_status | Shoulder test  | 3.5 |
| pdf_status | Report printing  | 3.6 |


### 3.8.1 Special instructions of interface:
Report printing interface, the report can be accessed when at least one of the measuring items are synthesized successfully 

**Others: **

+ When synthesis is unsuccessful or measurement of the item is not performed, the access interface will return, status code: 40006 

## 3.9 <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Retrieve Report History</font>


**<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Interface Description</font>****：**



+ <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Retrieve Report History</font>



**<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Request URL</font>****：**



+ `http://api.explorer.visbody.com/v1/history/data`



**<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Request Method</font>****：**



+ GET



**<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Parameters</font>****：**

| Parameter name  | Required  | Type  | Description  |
| :--- | :--- | :--- | --- |
| token | YES | string | Interface credential  |
| page | YES | string | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Page Number</font> |
| size | YES | string | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Page Number</font> |
| device_id | YES | string | Device ID |
| phone_number | No | string | Mobile phone No.  |
| start_data | No | string | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Start Time</font> |
| end_data | 否 | string | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">End Time</font> |




**<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Example Response</font>**



```plain
  {
  "code": 0,
  "data": {
    "scanIdArray": [
      "34041912080013-fe14541f-bd22-11eb-a249-300ed5517747",
      "34041912080013-034af912-bd22-11eb-a249-300ed5517747",
      "34041912080013-312ed677-bd1e-11eb-beeb-300ed5517747",
      "34041912080013-cadff1c4-bd1d-11eb-beeb-300ed5517747",
      "34041912080013-7e22e106-bd1d-11eb-beeb-300ed5517747",
      "34041912080013-804c181e-bd0e-11eb-beeb-300ed5517747",
      "34041912080013-338d2b02-bd0e-11eb-beeb-300ed5517747",
      "34041912080013-cea3e9a6-bd0c-11eb-beeb-300ed5517747",
      "34041912080013-8a634611-bd0c-11eb-beeb-300ed5517747",
      "34041912080013-737b5c04-bd09-11eb-beeb-300ed5517747"
    ],
    "count": 40
  }
}
```



<font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Response Parameter Description</font>

| <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Parameter Name</font><font style="color:rgb(38, 38, 38);">   </font> | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Type</font><font style="color:rgb(38, 38, 38);">   </font> | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Description</font><font style="color:rgb(38, 38, 38);">   </font> |
| --- | --- | --- |
| <font style="color:rgb(38, 38, 38);">scanIdArray</font><font style="color:rgb(38, 38, 38);">   </font> | <font style="color:rgb(38, 38, 38);">arrat</font><font style="color:rgb(38, 38, 38);">   </font> | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Return User History Data</font><font style="color:rgb(38, 38, 38);">   </font> |
| <font style="color:rgb(38, 38, 38);">count</font><font style="color:rgb(38, 38, 38);">   </font> | <font style="color:rgb(38, 38, 38);">number</font><font style="color:rgb(38, 38, 38);">   </font> | <font style="color:rgb(55, 65, 81);background-color:rgb(247, 247, 248);">Total Number of Records</font><font style="color:rgb(38, 38, 38);">   </font> |


<font style="color:rgb(38, 38, 38);">  
</font>

**  
**

