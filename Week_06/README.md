## 浏览器总论 ##
------
```seq
URL->HTML: HTTP  
HTML->DOM: parse
DOM->DOM with css: css computing
DOM width css->DOM with position: layout
DOM with position->Bitmap: render
```
## 有限状态机 ##
------
* 每一个状态都是一个机器
    * 在每一个机器里，我们可以做计算、存储、输出......
    * 所有的这些机器接受的输入是一致的
    * 状态机的每一个机器本身没有状态，如果我们用函数来表示的话，它应该是纯函数（无副作用）
* 每一个机器知道下一个状态
    * 每个机器都有确定的下一个状态（Moore）
    * 每个机器根据输入决定下一个状态（Mealy）
    ```javascript
    // JS中的有限状态机(Mealy）
    // 每个函数是一个状态
    function state(input) // 函数参数就是输入
    {
        // 在函数中，可以自由的编写代码，处理每个状态的逻辑
        return next; // 返回值作为下一个状态
    }
    
    // 以下是调用
    while(input){
        // 获取输入
        state = state(input); // 把状态机的返回值作为下一个状态
    }
    
    
    
    // 使用有限状态机处理字符串
    function match (str) {
        if (!str) {
            return false;
        }

        let state = start;
        for (let char of str) {
            state = state(char);
        }
        
        return state === end;
    }

    function start(c) {
        if (c === "a") {
            return foundA;
        } else {
            return start;
        }
    }

    function end(c) {
        return end;
    }

    function foundA(c) {
        if (c === "b") {
            return foundB;
        } else {
            return start(c);
        }
    } 

    function foundB(c) {
        if (c === "c") {
            return foundC;
        } else {
            return start(c);
        }
    } 

    function foundC(c) {
        if (c === "a") {
            return foundD;
        } else {
            return start(c);
        }
    } 

    function foundD(c) {
        if (c === "b") {
            return foundE;
        } else {
            return start(c);
        }
    } 

    function foundE(c) {
        if (c === "x") {
            return end;
        } else {
            return foundB(c);
        }
    } 

    console.log(match("test abcabx word"));
    ```
    
## HTTP请求 ##
--------
#HTTP的协议解析
------
* ISO-OSI七层网络模型
    | 七层        |  a   |    aa |
    | --------   | -----:  | :----:  |
    | 应用     | HTTP |  require("http")      |
    | 表示        |      |      |
    | 会话        |        |    |
    | 传输        |    TCP    | require("net") |
    | 网络        |    Internet    |    |
    | 数据链路        |    4G/5G/Wi-Fi    |    |
    | 物理层    |       |    |

* TCP与IP的一些基础知识
    * 流
    * 端口
    * require("net")
    * 包
    * IP地址
    * libnet/libpcap
* HTTP
    * Request
    * Response
* Http Request格式
    ```javascript
    POST / HTTP/1.1
    Host: 127.0.0.1
    Content-Type: application/x-www-form-urlencoded
    
    field1=aaa&code=x%3D1
    ```
* request代码
    ```javacript
    const http = require('http');

    http.createServer((request, response) => {
        let body = [];
        request.on('error', (err) => {
            console.log(err);
        }).on('data', (chunk) => {
            // body.push(chunk.toString);
            body.push(chunk);
        }).on('end', () => {
            body = Buffer.concat(body).toString();
            console.log("body:", body)
            response.writeHead(200, {'Content-Type': 'text/html'});
            response.end(" Hello World\n");
        })
    }).listen(8090);
    
    console.log("Server started!");
    ```
* response代码
    ```javacript
    const net = require('net');

    class Request {
        constructor(options) {
            this.method = options.method || 'GET';
            this.host = options.host;
            this.port = options.port || '80';
            this.path = options.path || '/';
            this.body = options.body || {};
            this.headers = options.headers || {};
    
            if (!this.headers["Content-Type"]) {
                this.headers["Content-Type"] = "application/x-www-form-urlencoded";
            }
    
            if (this.headers["Content-Type"] === "application/json") {
                this.bodyText = JSON.stringify(this.body);
            } else if (this.headers["Content-Type"] === "application/x-www-form-urlencoded") {
                this.bodyText = Object.keys(this.body).map((key) => {
                    return `${key}=${encodeURIComponent(this.body[key])}`;
                }).join("&");
            }
    
            this.headers["Content-Length"] = this.bodyText.length;
        }
    
        send(connection) {
            return new Promise((resolve, reject) => {
                let parser = new ResponseParser();
    
                if (connection) {
                    connection.write(this.toString());
                } else {
                    connection = net.createConnection({
                        host: this.host,
                        port: this.port
                    }, () => {
                        connection.write(this.toString());
                    })
                }
    
                connection.on("data", (data) => {
                    // console.log(data.toString());
                    parser.receive(data.toString());
                    if (parser.isFinished) {
                        resolve(parser.response);
                        connection.end();
                    }
                })
    
                connection.on("error", (err) => {
                    reject(err);
                    connection.end();
                })
            });
        }
    
        toString() {
            return `${this.method} ${this.path} HTTP/1.1\r
    ${Object.keys(this.headers).map(key => `${key}: ${this.headers[key]}`).join('\r\n')}\r
    \r
    ${this.bodyText}` 
        }
    }
    
    class ResponseParser {
        constructor() {
            this.WAITING_STATUS_LINE = 0;
            this.WAITING_STATUS_LINE_END = 1;
            this.WAITING_HEADER_NAME = 2;
            this.WAITING_HEADER_SPACE = 3;
            this.WAITING_HEADER_VALUE = 4;
            this.WAITING_HEADER_LINE_END = 5;
            this.WAITING_HEADER_BLOCK_END = 6;
            this.WAITING_BODY = 7;
    
            this.current = this.WAITING_STATUS_LINE;
            this.statusLine = "";
            this.headers = {};
            this.headerName = "";
            this.headerValue = "";
            this.bodyParser = null;
        }
    
        get isFinished() {
            return this.bodyParser && this.bodyParser.isFinished;
        }
    
        get response() {
            this.statusLine.match(/HTTP\/1.1 ([0-9]+) ([\s\S]+)/);
            return {
                statusCode: RegExp.$1,
                statusText: RegExp.$2,
                headers: this.headers,
                body: this.bodyParser.content.join('')
            }
        }
    
        receive(string) {
            for (let i=0; i<string.length; i++) {
                this.receiveChar(string.charAt(i));
            }
        }
    
        receiveChar(char) {
            if (this.current === this.WAITING_STATUS_LINE) {
                if (char === '\r') {
                    this.current = this.WAITING_STATUS_LINE_END;
                } else {
                    this.statusLine += char;
                }
            } else if (this.current === this.WAITING_STATUS_LINE_END) {
                if (char === '\n') {
                    this.current = this.WAITING_HEADER_NAME;
                }
            } else if (this.current === this.WAITING_HEADER_NAME) {
                if (char === ':') {
                    this.current = this.WAITING_HEADER_SPACE;
                } else if (char === '\r') {
                    this.current = this.WAITING_HEADER_BLOCK_END;
                    if (this.headers["Transfer-Encoding"] === "chunked") {
                        this.bodyParser = new TrunkBodyParser();
                    }
                } else{
                    this.headerName += char;
                }
            } else if (this.current === this.WAITING_HEADER_SPACE) {
                if (char === ' ') {
                    this.current = this.WAITING_HEADER_VALUE;
                }
            } else if (this.current === this.WAITING_HEADER_VALUE) {
                if (char === '\r') {
                    this.current = this.WAITING_HEADER_LINE_END;
                    this.headers[this.headerName] = this.headerValue;
                    this.headerName = "";
                    this.headerValue = "";
                } else {
                    this.headerValue += char;
                }
            } else if (this.current === this.WAITING_HEADER_LINE_END) {
                if (char === '\n') {
                    this.current = this.WAITING_HEADER_NAME;
                }
            } else if (this.current === this.WAITING_HEADER_BLOCK_END) {
                if (char === '\n') {
                    this.current = this.WAITING_BODY;
                }
            } else if (this.current === this.WAITING_BODY) {
                this.bodyParser.receiveChar(char);
            }
        }
    }
    
    class TrunkBodyParser {
        constructor() {
            this.WAITING_LENGTH = 0;
            this.WAITING_LENGTH_LINE_END = 1;
            this.READING_TRUNK = 2;
            this.WAITING_NEW_LINE = 3;
            this.WAITING_NEW_LINE_END = 4;
            this.length = 0;
            this.content = [];
            this.isFinished = false;
            this.current = this.WAITING_LENGTH;
        }
    
        receiveChar(char) {
            if (this.current === this.WAITING_LENGTH) {
                if (char === '\r') {
                    if (this.length === 0) {
                        this.isFinished = true;
                    }
                    this.current = this.WAITING_LENGTH_LINE_END;
                } else {
                    this.length *= 16;
                    this.length += parseInt(char, 16);
                }
            } else if (this.current === this.WAITING_LENGTH_LINE_END) {
                if (char === '\n') {
                    this.current = this.READING_TRUNK;
                }
            } else if (this.current === this.READING_TRUNK) {
                this.content.push(char);
                this.length--;
                if (this.length === 0) {
                    this.current = this.WAITING_NEW_LINE;
                }
            } else if (this.current === this.WAITING_NEW_LINE) {
                if (char === '\r') {
                    this.current = this.WAITING_NEW_LINE_END;
                }
            } else if (this.current === this.WAITING_NEW_LINE_END) {
                if (char === '\n') {
                    this.current = this.WAITING_LENGTH;
                }
            }
        }
    }
    
    void async function() {
        let request = new Request({
            method: "POST",
            host: "127.0.0.1",
            port: "8090",
            path: "/",
            headers: {
                ["X-Foo2"]: "customed"
            },
            body: {
                name: "winter"
            }
        });
    
        let response = await request.send();
        console.log(response);
    }()
    ```
    
