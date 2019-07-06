```python
#!/home/homework/jingqi/envs/anaconda2/bin/python
# coding=utf-8

import requests
import json
import sys

reload(sys)
sys.setdefaultencoding('utf-8')

statusMap = {"NEW": "NEW", "PENDING": "PENDING", "RUNNING": "RUNNING", "ERROR": "ERROR"}
typesMap = {"BUILD": "BUILD", "MERGE": "MERGE"}
timeStandard = {"BUILD": 270, "MERGE": 420}


def json2Tuple(x):
    return x['name'], x['job_status'], x['duration']


def sendDing(resTuples, msg):
    url = 'xxxurl'
    headers = {"Content-type": "application/json", "charset": "utf-8"}
    dingMsg = {"msgtype": "text", "at": {"atMobiles": ["xxx"], "isAtAll": False}}
    resTuples['msg'] = msg
    dingMsg['text'] = {
        "content": json.dumps(resTuples, sort_keys=True,
                              indent=4, separators=(',', ': '), ensure_ascii=False) # .encode("utf-8")}
    requests.post(url, data=json.dumps(dingMsg), headers=headers)


def getWarnDuraJobs(jobs):
    buildJobs = typiJobs(jobs, typesMap['BUILD'])
    return filter(lambda x: x[2] >= timeStandard['BUILD'], buildJobs)


def typiJobs(jobs, typi):
    if typi == typesMap['BUILD'] or typi == typesMap['MERGE']:
        return filter(lambda x: x[0].startswith(typi), jobs)
    elif typi == statusMap['ERROR']:
        return filter(lambda x: x[1] == typi, jobs)
    else:
        return []


def resultTuples(jobs):
    # ($name, $job_status, $duration)
    numJobs = len(jobs)
    numBuilds = len(typiJobs(jobs, typesMap['BUILD']))
    numMerges = len(typiJobs(jobs, typesMap['MERGE']))
    warnJobsNum = numJobs
    vWarnDuraJobs = getWarnDuraJobs(jobs)
    fatalStatusJobs = typiJobs(jobs, statusMap['ERROR'])
    return {"warnJobsNum": warnJobsNum, "numBuilds": numBuilds, "numMerges": numMerges,
            "vWarnDuraJobs": vWarnDuraJobs, "fatalStatusJobs": fatalStatusJobs}


def getJsonObj():  # [{},{}]
    url1 = "http://ip:port/kylin/api/jobs"
    auth = ('ADMIN', 'KYLIN')
    params = [("projectName", "realtime_flow"),
              # ("cubeName","cube_kylin_realtime_flow_official_check"),
              ("status", 0),
              ("status", 1),
              ("status", 2),
              ("status", 8),
              ("offset", 0),
              ("limit", 100),
              ("timeFilter", 1),
              ("jobSearchMode", "ALL")]
    result = requests.request("get", url1, params=params, auth=auth)
    jsonObj = result.json()
    return jsonObj


def judge(resTuples):
    if resTuples['warnJobsNum'] >= 5:
        msg = "加工任务过多"
        sendDing(resTuples, msg)
    if len(resTuples['vWarnDuraJobs']) > 0:
        msg = "加工时间过长"
        sendDing(resTuples, msg)
    if len(resTuples['fatalStatusJobs']) > 0:
        msg = "任务加工失败"
        sendDing(resTuples, msg)


if __name__ == '__main__':
    jsonObj = getJsonObj()
    jobs = map(json2Tuple, jsonObj)
    resTuples = resultTuples(jobs)
    judge(resTuples)

```
