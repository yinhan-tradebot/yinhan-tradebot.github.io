# Optimizer
## 1. Upload
- api: https://wss.frontline-optimizer.com/upload
    - method: POST
    - request:
        - body:
            - file: file to be uploaded（文件名中不要有特殊字符，且只能包含后缀前的那1个点号，需要前端处理下）
    - response: json
        - example:
            ```json
            {
                "code": 0,
                "message": "Success",
                "originalDurationDaysWithCalendar": 1, // plan总工期
                "maxResourceUnitAgg": 0,
                "newCriticalTasksLen": 4,
                "mapping": {
                    "testwbs.xml": "6dfa1bc0c7f4c789980701500.xml" // 后端返回的文件名映射
                },
                "milestoneTasksInfo": [], // 对优化有影响的里程碑任务信息
                "predecessorSummaryInfo": [], // 对优化有影响的前置Summary任务信息
                "loginAndauthRequired": false // 任务数量是否超过1000限制
            }
            ```

- api: https://wss.frontline-optimizer.com/upload
    - method: POST
    - request:
        - body:
            - file: xlsx file to be uploaded
            - newName: 前一个接口上传项目文件时返回的文件名映射中的文件名，如："6dfa1bc0c7f4c789980701500.xml" 或 "6dfa1bc0c7f4c789980701500.xlsx" 或 "6dfa1bc0c7f4c789980701500"
    - response: json
        - example:
            ```json
            {
                "code": 0,
                "message": "Success"
            }
            ```
## 2. Constraints Download
 - api: https://wss.frontline-optimizer.com/fileDownload/constraints/**{file_name}**.xlsx
    - file_name: 上传文件返回的文件名映射中的文件名（无后缀）
    - method: GET
    - response: xlsx file
    - example:
        - 6dfa1bc0c7f4c789980701500.xml
        - https://wss.frontline-optimizer.com/fileDownload/constraints/6dfa1bc0c7f4c789980701500.xlsx



## 3. Websocket
- api: wss://wss.frontline-optimizer.com/websockets
    - method: WebSocket
    - request: json
        - example:
            ```json
            {
                "fileName": "82240e40104d8cdd263a68104.xer", // 上传文件返回的文件名映射中的文件名（加后缀）
                "size": 0.001, // 上传文件大小
                "setting": {
                    "resourceConstraint": false, // 资源约束的调整方式，false表示调整工期，"lag"表示调整延迟，"logic"表示调整逻辑关系
                    "considerDefaultResourceType": false, // 考虑默认资源类型，false表示不考虑，"any"表示无资源的任务分配默认资源，“all”表示当所有任务都无资源时分配默认资源
                    "considerActualDates": true, // 是否考虑实际日期
                    "considerDataDate": true, // 是否考虑数据日期
                    "optimizationSteps": 10,
                    "learningRate": 0.025,
                    "minDurationRatio": 0.5,
                    "maxDurationRatio": 10.0
                }
            }
            ```
    - response: json
        - connection success
            ```json
            {
                "code": 0,
                "message": "Success"
            }
            ```
        - optimization result
            - Continuously return the following JSON data
                ```json
                {
                    "name":"Fastest",
                    "result":{
                        "step":0, /* step为0表示baseline */
                        "loss":1,
                        "constraintLoss":0,
                        "planDurationDays":0.375,
                        "projectDurationDays":0.375,
                        "baselineDurationDays":0.375,
                        "durationsPercentage":0,
                        "projectDurationDaysWithCalendar":1, /* 项目最新工期 */"baselineDurationDaysWithCalendar":1, /* 项目基线工期 */"durationsPercentageWithCalendar":0,
                        "group":"baseline",
                        "totalResourceCount":0,
                        "maxResourceUnitAgg":0, /* 最大Labor资源使用量（聚合） */
                        "minResourceUnitAgg":0,
                        "spanResourceUnitAgg":0, /* Labor资源使用量跨度（聚合） */
                        "maxResourceUnit":0,
                        "minResourceUnit":0,
                        "maxMaterialUnit":0,
                        "minMaterialUnit":0,
                        "totalCost":0,
                        "currSymbol":"¥",
                        "currName":"CNY",
                        "changedTasksLen":0,
                        "baselineTasksLen":8,
                        "newCriticalTasksLen":4,
                        "baselineCriticalTasksLen":4,
                        "criticalPercentage":0
                    }
                }
                ```
        - optimization finished
            ```json
            {
                "code":0,
                "message":"Done"
            }
            
            ```

## 4. Result
- api: https://wss.frontline-optimizer.com/results
    - method: GET
    - request:
        - params:
            - preset: "Fastest" // 优化预设名称
            - fileName: "6dfa1bc0c7f4c789980701500.xml" // 上传文件返回的文件名映射中的文件名(加后缀)
            - step: 5 // 步数，0表示baseline
            - considerDefaultResourceType: false // 考虑默认资源类型(与websocket中的setting.considerDefaultResourceType一致)
            - resourceConstraint: false // 资源约束的调整方式(与websocket中的setting.resourceConstraint一致)
    - response: json/gzip
        - 说明：根据响应头判断返回的数据类型，如果是json，则为json格式，如果是gzip，则为压缩格式，需要解压后才能得到json数据。（之前为了将大量数据压缩，少量数据直接传输）
    - example:
        - https://wss.frontline-optimizer.com/results?preset=Fastest&fileName=6dfa1bc0c7f4c789980701500.xml&step=5&considerDefaultResourceType=false&resourceConstraint=false

## 5. report xlsx/original format Download
- api: https://wss.frontline-optimizer.com/fileDownload/reports/**{preset_name}**-**{file_name}**_FrontlineExport.**{suffix}**
    - method: GET
    - preset_name: 优化预设名称
    - file_name: 上传文件返回的文件名映射中的文件名（无后缀）
    - suffix: xlsx/original format
    - response: xlsx file/original format file

    - example:
        - Fastest
        - 6dfa1bc0c7f4c789980701500.xml
        - xlsx
        - https://wss.frontline-optimizer.com/fileDownload/reports/Fastest-6dfa1bc0c7f4c789980701500_FrontlineExport.xlsx

## 6. Authentication
- api: https://wss.frontline-optimizer.com/auth
    - method: GET
    - request:
        - params:
            - user: 用户名(邮箱地址)
    - response: json
        - example:
            ```json
            {
                "code": 0,
                "message": "Authorized",
                "auth": true
            }
            ```
            OR
            ```json
            {
                "code": 0,
                "message": "Unauthorized",
                "auth": false
            }
            ```

