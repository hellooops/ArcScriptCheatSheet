# Arc Script Cheat Sheet

## Keywords
### Get & Set
```xml
<arc:set attr="tmp.name" value="Mike" />
<arc:set item="tmp" attr="name2" value="Mike"/>
<arc:set attr="_.name" value="Mike" />
<arc:set attr="name" value="Mike" />  <!-- Same as _.name -->
<arc:set attr="name">Mike</arc:set>
<arc:setc attr="xpath" value="/root/book[1]/@name\<>()" />  <!-- Won't escape special characters like [] -->

<arc:setm>
name = ContactName 
company = ContactCompany
</arc:setm>
<arc:setm item="someone">
name = ContactName
company = ContactCompany
</arc:setm>

<arc:set attr="tmp.a" value="a"/>
<arc:unset item="tmp" attr="a"/>
<arc:unset attr="tmp.a"/>

<arc:set attr="a.name" value="name"/>
<arc:set attr="a.age1" value="age1"/>
<arc:set attr="a.age2" value="age2"/>
<arc:unset attr="a.age*"/>  <!-- Remove a.age1 and a.age2 -->

[*]
[someone.*]
[someone.name]

<arc:set attr="item.space:attrName" value="1"/>
[item.space:attrName]
```
### Array
```xml
<arc:set attr="testArray#" value="10"/>
<arc:set attr="testArray#" value="20"/>
<arc:set attr="testArray#" value="30"/>
[testArray]: Error
[testArray#]  <!-- Count: 3 -->
[testArray#1]  <!-- Index starts from 1 -->
[testArray#4]: Error
[testArray | implode(",")]  <!-- 10,20,30 -->
[testArray | join(",")]  <!-- 10,20,30 -->

<arc:enum attr="testArray" expand="true">
  ([_index]) [_attr] -> [_value]
  <!-- (1) testArray#1 -> 10 (2) testArray#2 -> 20 (3) testArray#3 -> 30 -->
</arc:enum>

<arc:check attr="testArray" />  Error
<arc:check attr="testArray#" />
<arc:check attr="testArray#1" />

```
### If else
```xml
<arc:set attr="age" value="10"/>

<arc:if attr="age" operator="null">null</arc:if>
<arc:if attr="age" operator="notnull">notnull</arc:if>
<arc:if attr="age" operator="hasvalue">hasvalue</arc:if>
<arc:if exp="[age] == 10">equals if</arc:if>
<arc:if attr="age" value="10" operator="equals">equals</arc:if>
<arc:if attr="age" value="10" operator="equalsignorecase">equalsignorecase</arc:if>
<arc:if exp="[age] != 20">notequals if</arc:if>
<arc:if attr="age" value="20" operator="notequals">notequals</arc:if>
<arc:if exp="[age] < 20">lessthan if</arc:if>
<arc:if attr="age" value="20" operator="lessthan">lessthan</arc:if>
<arc:if exp="[age] > 1">greaterthan if</arc:if>
<arc:if attr="age" value="1" operator="greaterthan">greaterthan</arc:if>
<arc:if exp="[age] <= 100">lessthanequals if</arc:if>
<arc:if exp="[age] >= 10">greaterthanequals if</arc:if>

<arc:if exp="[age] == 10">
  Equals
  <arc:else>
    Not Equals
  </arc:else>
</arc:if>

<arc:if attr="age" value="10" operator="equals">
  Equals
  <arc:else>
    Not Equals
  </arc:else>
</arc:if>

<!-- Compare string -->
<arc:set attr="name" value="abc"/>
<arc:if exp="[name] == 'abc'">abc</arc:if>  Error: Unable to convert the value [abc] to a double.
<arc:if attr="name" value="abc" operator="equals">ok</arc:if>
<arc:if attr="name" value="aba" operator="greaterthan">compare string</arc:if>
```
### Loop
```xml
<arc:set item="input" attr="Greeting" value="Hello" />
<arc:set item="input" attr="Goodbye" value="See ya" />
<arc:enum item="input">[_attr] is [_value]</arc:enum>
<arc:enum attr="input.*">[_attr] is [_value]</arc:enum>
<arc:enum attr="input.Good*">[_attr] is [_value]</arc:enum>

<arc:set attr="colors" value="violet,indigo,blue,green,yellow, orange,red"/>
<arc:enum list="[colors]" separator=",">[_value]</arc:enum>

<arc:enum range="a..z">[_value]</arc:enum>
<arc:enum range="Z..A">[_value]</arc:enum>
<arc:enum range="1..25">[_value]</arc:enum>

<arc:set attr="foo.email#1" value="joe@mycompany.com"/>
<arc:set attr="foo.email#2" value="john@mycompany.com"/>
<arc:enum attr="foo.email" expand="true">([_index]) [_attr] -> [_value]</arc:enum>
```
### Try catch
```xml
<arc:try>
  <arc:throw code="myerror" description="thedescription" details="Other Details."/>
  <arc:catch code="myerror">
    catch
    <arc:set attr="arc:ecode" value="[_code]"/>
    <arc:set attr="arc:emessage" value="[_description]: [_details]"/>
    <arc:push/>
  </arc:catch>
  <arc:finally>
    finally
  </arc:finally>
</arc:try>
```
### Check null and empty
```xml
<arc:set attr="tmp" value="false"/>
<arc:check attr="tmp">
  true
  <arc:else>
    false
  </arc:else>
</arc:check>

<arc:check attr="NotExists">false</arc:check>
<arc:check value="11">true</arc:check>
<arc:check value="">false</arc:check>
<arc:check value=" ">true</arc:check>
<arc:check value="false">true</arc:check>
<arc:check value="0">true</arc:check>

<arc:exists attr="tmp">true</arc:exists>
<arc:exists attr="NotExists">false</arc:exists>
<arc:null attr="NotExists">true</arc:null>
<arc:notnull attr="tmp">true</arc:notnull>

<arc:set attr="prop1" value="value1"/>
<arc:set attr="prop2" value="value2"/>
<arc:set attr="prop4" value="value4"/>
<arc:enum list="prop1,prop2,prop3,prop4" separator=",">
  <arc:null attr="[_value]" action="break" />
  [_value]
</arc:enum>
```
### Map
```xml
<arc:set attr="item1.var1" value="x"/>
<arc:set attr="item1.var2" value="y"/>
<arc:set attr="item2.attr1" value="z"/>
<arc:map from="item1" to="item2" map="attr1=var1, attr2=var2"/>
<arc:map from="item1" to="item2" map="*=*"/>
[item2.*]
<!-- attr1	x
attr2	y
var1	x
var2	y -->

<arc:set attr="item3.space1:var1" value="a"/>
<arc:set attr="item3.space1:var2" value="b"/>
<arc:set attr="item3.space2:var1" value="m"/>
<arc:set attr="item3.attr1" value="z"/>
<arc:map from="item3" to="item3" map="space2:*=space1:*"/>

<!-- attr1	z
space1:var1	a
space1:var2	b
space2:var1	a
space2:var2	b -->
[item3.*]
```
### Function
There are 2 ways to define a function
#### Call script as function
Define functions
```xml
<!-- utils.rsb -->
<arc:null attr="_input.meta:method">
  <arc:throw code="MethodNotSet" desc="Invoke utils.rsb without method set." />
</arc:null>

<arc:select value="[_input.meta:method]">
  <arc:case value="sum">
    <arc:set attr="output.result" value="0"/>
    <arc:enum attr="_input.args:*">
      <arc:set attr="output.result" value="[output.result|add([_value])]"/>
    </arc:enum>
    <arc:push item="output"/>
    <arc:break/>
  </arc:case>
  <arc:case value="fabonacci">
    <arc:if exp="[_input.args:n] == 1">
      <arc:set attr="output.result" value="1"/>
      <arc:else>
        <arc:if exp="[_input.args:n] == 2">
          <arc:set attr="output.result" value="1"/>
          <arc:else>
            <arc:set attr="fabonacciIn.meta:method" value="fabonacci"/>

            <arc:set attr="fabonacciIn.args:n" value="[_input.args:n | subtract(1)]"/>
            <arc:call op="utils.rsb" in="fabonacciIn" out="fabonacciOut">
              <arc:set attr="tmp.result1" value="[fabonacciOut.result]"/>
            </arc:call>

            <arc:set attr="fabonacciIn.args:n" value="[_input.args:n | subtract(2)]"/>
            <arc:call op="utils.rsb" in="fabonacciIn" out="fabonacciOut">
              <arc:set attr="tmp.result2" value="[fabonacciOut.result]"/>
            </arc:call>

            <arc:set attr="output.result" value="[tmp.result1 | add([tmp.result2])]"/>
          </arc:else>
        </arc:if>
      </arc:else>
    </arc:if>
    <arc:push item="output"/>
    <arc:break/>
  </arc:case>
  <arc:default>
    <arc:throw code="MethodNotFound" desc="Could not find method [_input.meta:method]" />
  </arc:default>
</arc:select>
```

Call function
```xml
<arc:set attr="sumIn.args:a" value="11"/>
<arc:set attr="sumIn.args:b" value="2"/>
<arc:set attr="sumIn.args:c" value="100"/>
<arc:set attr="sumIn.meta:method" value="sum"/>
<arc:call op="utils.rsb" in="sumIn" out="sumOut">
  [sumOut.*]
</arc:call>


<arc:enum range="1..5">
  <arc:set attr="fabonacciIn.args:n" value="[_value]"/>
  <arc:set attr="fabonacciIn.meta:method" value="fabonacci"/>
  <arc:call op="utils.rsb" in="fabonacciIn" out="fabonacciOut">
    [fabonacciOut.result]
  </arc:call>
</arc:enum>

```

#### use arc:render as function
```xml
<arc:setc attr="utils.sum" value="[a|add([b])]"/>

<arc:set attr="sumIn.a" value="1"/>
<arc:set attr="sumIn.b" value="2"/>
<arc:render templateData="[utils.sum]" in="sumIn" to="output1.result" />
[output1.result]


<arc:setc attr="utils.sum2">
  <!-- Note: The code here need to be escaped. -->
  <!--
  <arc:set attr="tmp.a" value="[a]"/>
  <arc:set attr="tmp.b" value="[b]"/>
  <arc:set attr="tmp.c" value="[tmp.a|add([tmp.b])]"/>
  -->
  &lt;arc:set attr=&quot;tmp.a&quot; value=&quot;[a]&quot;/&gt;
  &lt;arc:set attr=&quot;tmp.b&quot; value=&quot;[b]&quot;/&gt;
  &lt;arc:set attr=&quot;tmp.c&quot; value=&quot;[tmp.a|add([tmp.b])]&quot;/&gt;
  [tmp.c]
</arc:setc>

<arc:render templateData="[utils.sum2]" in="sumIn" to="output2.result" />
[output2.result]
```


### Logs
```xml
<arc:set attr="_log.info">info</arc:set>
<arc:set attr="_log.warning">warning</arc:set>
<arc:set attr="_log.error">error</arc:set>
```
### call
Call Ops
```xml
<arc:set attr="fileListDirIn.path" value="C:\\"/>
<arc:call op="fileListDir" in="fileListDirIn" out="output">
  [output.file:name]
</arc:call>
```

Call Script
```xml
<!-- script1.rsb -->
<arc:set attr="result.name" value="Deric"/>
<arc:push item="result"/>

<!-- script2.rsb -->
<arc:call op="script1.rsb" out="out">
  [out.*]
</arc:call>
```



### push
```xml
<arc:set attr="output.name" value="Deric"/>
<arc:push item="output" />
```

## Ops
### dbBeginTransaction
### dbCall
### dbEndTransaction
### dbListColumns
### dbListTables
### dbListViews
### dbNonQuery
### dbQuery
### fileCopy
### fileCreate
### fileDelete
### fileListDir
### fileMakeDir
### fileMove
### fileRead
### fileReadLine
### fileReceive
### fileWrite
### httpGet
### httpPost
### httpPut
### httpReceive
### httpUpload
### sysExecute
### xmlClose
### xmlDOMGet
### xmlDOMSearch
### xmlGet
### xmlOpen
### xmlPut
### zipCompress
### zipExtract
### zipScan

## Formatters
### String
### Date
### Number
### File
### Others
