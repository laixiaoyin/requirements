# 说明

## DataServer 登录

### DataServer_01
```
chmod 600 DataServer_01.pem
ssh -i "DataServer_01.pem" ubuntu@ec2-18-140-101-226.ap-southeast-1.compute.amazonaws.com -p 22
```
### DataServer_02
```
chmod 600 DataServer_02.pem
ssh -i "DataServer_02.pem" ubuntu@ec2-18-140-76-46.ap-southeast-1.compute.amazonaws.com -p 22
```
### DataServer_03
```
chmod 600 DataServer_03.pem
ssh -i "DataServer_03.pem" ubuntu@ec2-18-136-252-105.ap-southeast-1.compute.amazonaws.com -p 22
```

## MongoDB配置

### 副本集

```json
{
	"set" : "IDHub",
	"date" : ISODate("2019-08-27T06:37:07.902Z"),
	"myState" : 1,
	"term" : NumberLong(2),
	"heartbeatIntervalMillis" : NumberLong(2000),
	"members" : [
		{
			"_id" : 1,
			"name" : "18.140.101.226:27017",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 77,
			"optime" : {
				"ts" : Timestamp(1566887824, 1),
				"t" : NumberLong(2)
			},
			"optimeDate" : ISODate("2019-08-27T06:37:04Z"),
			"electionTime" : Timestamp(1566887762, 1),
			"electionDate" : ISODate("2019-08-27T06:36:02Z"),
			"configVersion" : 1,
			"self" : true
		},
		{
			"_id" : 2,
			"name" : "18.140.76.46:27017",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 66,
			"optime" : {
				"ts" : Timestamp(1566887824, 1),
				"t" : NumberLong(2)
			},
			"optimeDurable" : {
				"ts" : Timestamp(1566887824, 1),
				"t" : NumberLong(2)
			},
			"optimeDate" : ISODate("2019-08-27T06:37:04Z"),
			"optimeDurableDate" : ISODate("2019-08-27T06:37:04Z"),
			"lastHeartbeat" : ISODate("2019-08-27T06:37:06.784Z"),
			"lastHeartbeatRecv" : ISODate("2019-08-27T06:37:07.587Z"),
			"pingMs" : NumberLong(0),
			"syncingTo" : "18.136.252.105:27017",
			"configVersion" : 1
		},
		{
			"_id" : 3,
			"name" : "18.136.252.105:27017",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 63,
			"optime" : {
				"ts" : Timestamp(1566887824, 1),
				"t" : NumberLong(2)
			},
			"optimeDurable" : {
				"ts" : Timestamp(1566887824, 1),
				"t" : NumberLong(2)
			},
			"optimeDate" : ISODate("2019-08-27T06:37:04Z"),
			"optimeDurableDate" : ISODate("2019-08-27T06:37:04Z"),
			"lastHeartbeat" : ISODate("2019-08-27T06:37:06.801Z"),
			"lastHeartbeatRecv" : ISODate("2019-08-27T06:37:07.248Z"),
			"pingMs" : NumberLong(1),
			"syncingTo" : "18.140.101.226:27017",
			"configVersion" : 1
		}
	]
}
```

### 用户

* `root`
* `dbAdminAnyDatabase`
* `userAdminAnyDatabase`
* `readWriteAnyDatabase`
