<h1>WebSocket API protocol</h1>
- websocket url : wss://ws.25hrbanking.io/

<h2>**** REQUEST/REPLY/RESPONSE ****</h2>

<h3>#Request body (Content-Type ==> application/json ).</h3>
<h5>TYPE REQUEST</h5>
<pre><code>{

    "method": "METHOD_TO_REQUEST",   +// method [type sting]
    "params": [],  +// method parameter  [type json array]
    "id": 1  +// request id [type integer]
}</code></pre>

<h3>#Reply on request.</h3>
<h5>TYPE REPLY</h5>
<pre><code>{
    "error": null,  // error code and message. null for success, non-null for failure.  [type json object]
    "result": "", //   reply  result   /[type any]
    "id": 1  //response id from request id [type integer]
}</code></pre>



<h3>#Response notify method body.</h3>
<h5>TYPE NOTIFY</h5>
<pre><code>{
    "method": "METHOD_TO_RESPONSE", // response method [type sting]
    "params": [],  // result parameter  [type json array]
    "id": null //response id from request id or null [type integer]
}</code></pre>


<br></br><h2>**** REQUEST METHOD ****</h2>
<h1><h1>
<h3>@@@@ System API @@@@</h3>

<h4>#SERVER PING / PONG </h4>

<pre><code>
<h5>method name = "server_ping"
response type = REPLY<p></p>
example request: </h5>
{
    "method": "server_ping",
    "params": [],
    "id": 1000
}


<h5>example reply: </h5>
{
    "error": null,
    "result": "pong",
    "id": 1000
}
</pre></code>

<h4>#SERVER TIME</h4>

<pre><code>
        <h5>    
method name = "server_time"
response type = REPLY<p></p>
example request: 
        </h5>
{
    "method": "server_time",
    "params": [],
    "id": 1000
}


<h5>example reply: </h5>
{
    "error": null,
    "result": 1568606458,
    "id": 1000
}
</pre></code>

<h3>@@@@ MARKET API @@@@</h3>

<h4>#SUBSCRIBE KLINE CHART</h4>
<pre><code>
        <h5>
method name = "chart_start"
response type = REPLY,NOTIFY<p></p>
example request: </h5>
{
    "method": "chart_start",
    "params": ["SYMBOL",INTERVAL],  //["BTC_USDT",5]
    "id": 1000
}


<h5>example reply: </h5>
{
    "error": null,
    "result": {
        "status": "success"
    },
    "id": 1000
}

<h5>example notify : </h5>
{
    "method": "chart_update",
    "params": [
        {
            "time": 1568607145,
            "close": "10300",
            "open": "10300",
            "high": "10300",
            "low": "10300",
            "volume": "3"
        }
    ],
    "id": null
}
</pre></code>

<h4>#SUBSCRIBE ORDER DEPTH DATA & CHART</h4>
<pre><code>
        <h5>
method name = "depth_start"
response type = REPLY,NOTIFY<p></p>
example request: </h5>
{
    "method": "depth_start",
    "params": ["SYMBOL",LIMIT COUNT ,"MERGE INTERVAL"],  //["BTC_USDT",20,"0"]
    "id": 1000
}

<h5>limit: [1, 5, 10, 20, 30, 50, 100],
merge interval: ["0", "0.00000001", "0.0000001", "0.000001", "0.00001", "0.0001", "0.001", "0.01", "0.1"]
</h5>


<h5>example reply: </h5>
{
    "error": null,
    "result": {
        "status": "success"
    },
    "id": 1000
}

<h5>example notify : </h5>
{
    "method": "depth_update",
    "params": [
        true,
        {
            "asks": [
                [
                    "10350",
                    "0.8"
                ]
            ],
            "bids": [
                [
                    "10300",
                    "127"
                ],
                [
                    "10100",
                    "60"
                ]
            ]
        },
        "BTC_USDT"
    ],
    "id": null
}
</pre></code>

<h4>#SUBSCRIBE SYMBOL PRICE DATA</h4>

<pre><code>

<h5>method name = "price_start"
response type = REPLY,NOTIFY<p></p>
example request:</h5>
{
    "method": "price_start",
    "params": ["SYMBOL"]   // symbol array list or "ALL"  ex : ["BTC_USDT","ETH_USDT","LTC_USDT"]  or  ["ALL"]
    "id": 1000
}



<h5>example reply: </h5>
{
    "error": null,
    "result": {
        "status": "success"
    },
    "id": 1
}

<h5>example notify : </h5>
{
    "method": "price_update",
    "params": [
        "BTC_USDT",
        {
            "price": "10300",
            "change": "0"
        }
    ],
    "id": null
}
</pre></code>



<h4>#SUBSCRIBE SYMBOL OVERVIEW STATE DATA</h4>
<pre><code>
        <h5>
method name = "pair_state_start"
response type = REPLY,NOTIFY<p></p>
example request: 
</h5>
{
    "method": "pair_state_start",
    "params": ["BTC_USDT"],  // symbol array list or "ALL"  ex : ["BTC_USDT","ETH_USDT","LTC_USDT"]  or  ["ALL"]
    "id": 1
}



<h5>example reply: </h5>
{
    "error": null,
    "result": {
        "status": "success"
    },
    "id": 1
}

<h5>example notify : </h5>
{
    "method": "pair_state_update",
    "params": [
        "BTC_USDT",
        {
            "pair": "BTC_USDT",
            "last": "10300",
            "open": "10300",
            "close": "10300",
            "high": "10300",
            "low": "10300",
            "volume": "3",
            "deal": "30900",
            "change": "0.00"
        }
    ],
    "id": null
}

</pre></code>


<h4>#SUBSCRIBE SYMBOL TODAY STATE DATA</h4>

<pre><code>
    
<h5>method name = "today_state_start"
response type = REPLY,NOTIFY<p></p>
example request: 
</h5>
{
    "method": "today_state_start",
    "params": ["BTC_USDT"],  // symbol array list or "ALL"  ex : ["BTC_USDT","ETH_USDT","LTC_USDT"]  or  ["ALL"]
    "id": 1
}

<h5>
example reply: </h5>
{
    "error": null,
    "result": {
        "status": "success"
    },
    "id": 1
}
<h5>
example notify : </h5>
{
    "method": "today_update",
    "params": [
        "BTC_USDT",
        {
            "open": "10300",
            "last": "10300",
            "high": "10300",
            "low": "10300",
            "change": "0.00",
            "volume": "3",
            "deal": "30900"
        }
    ],
    "id": null
}
</pre></code>


<h4>#SUBSCRIBE TRADE HISTORY</h4>
<pre><code>
<h5>method name = "trade_history_start"
response type = REPLY,NOTIFY<p></p>
example request: </h5>
{
    "method": "trade_history_start",
    "params": ["BTC_USDT"],
    "id": 1
}
<h5>

example reply: </h5>
{
    "error": null,
    "result": {
        "status": "success"
    },
    "id": 1
}

<h5>example notify : </h5>
{
    "method": "trade_history_update",
    "params": [
        "BTC_USDT",
        [
            {
                "id": 284576,
                "time": 1568607147.179158,
                "price": "10300",
                "amount": "3",
                "type": "buy"
            },
            {
                "id": 284575,
                "time": 1567657084.945448,
                "price": "10350",
                "amount": "0.1",
                "type": "buy"
            },
            {
                "id": 284574,
                "time": 1567656751.577719,
                "price": "10350",
                "amount": "0.1",
                "type": "buy"
            },
            {
                "id": 284573,
                "time": 1567656714.015983,
                "price": "10350",
                "amount": "0.9",
                "type": "buy"
            }
        ]
    ],
    "id": null
}
</pre></code>
