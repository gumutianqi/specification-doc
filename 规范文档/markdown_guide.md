# Markdown 规范


## `md语法`

### table的支持

<table class="tg">
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <tr>
    <td>ddd</td>
    <td>ddd</td>
    <td>
      <p>
       ppp
        <a href="https://botbot.me/freenode/docker/#" target="_blank">link</a>.
      </p>
    </td>
  </tr>
  <tr>
    <td>G我们的代码</td>
    <td>ddd</td>
    <td>
      cxn
    </td>
  </tr>
</table>

### 另一种
| **Java** (Linux) | **Js** (linux) | **Hkr** | **Reachauto** |
|-------------|---------------|----|------------|
| demo | demo2 | demo3 | demo4 |
| demo | demo2 | demo3 | demo4 |


## xxxxxxx

> 引用 xxxxxxxxxxxxx b xxxxxxxxxxxxx

  * *xxxxx*: xxxxx.
  * *xxxxx*: xxxxxx.
  * *xxx*: xxxxxxx.
  * *xxx-xxx*: xxxxxxxx
1.  *xxxxx*: xxxxx.
2.  *xxxxx*: xxxxxx.
3.  *xxx*: xxxxxxx.
4.  *xxx-xxx*: xxxxxxxx

## code
```java
package com.reachauto.hkr.exception;

/**
 * 输入参数异常
 */
public class InputParameterException extends HkrClientException {

    private static final long serialVersionUID = -2030641041470839173L;
    private final static int CODE = 4001;//输入参数异常

    public InputParameterException(String msg) {
        super(CODE, msg);
    }

    public InputParameterException(String msg, Throwable cause) {
        super(msg, cause);
    }
}
```
![示例](211438200988939.png)