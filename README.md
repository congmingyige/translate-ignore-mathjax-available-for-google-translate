# translate-ignore-mathjax-available-for-google-translate
谷歌翻译 忽略mathjax公式的翻译

- 可以使用Tampermonkey
- 添加网页，比如https://journals.sagepub.com/* ， 代表支持 https://journals.sagepub.com 旗下的网址
// @match        https://journals.sagepub.com/*

# power automate 流 : 多篇文献浏览器翻译，包括浏览器滚动、页面自动转移，公式、变量不翻译（一些设置）等

有多少个网址，就添加多少个网址到List_website。注意顺序，它们分别位于窗口的第1、2、3、……、n个位置，使可以用Ctrl+1、2、3、……、n到达这个网页所在位置

power automate对于少部分网页，无法运行，到了某一步，会卡死，对于这个网页就不要用power automate。还有有可能MathJax_SVG对应的javascript在Power Automate里跑不通，那么禁用这条语句，然后在网页刷新后，控制台执行这段语句，我印象中temportary money好像不太行。

javascript输入。根据网页具体情况来修改，有时候难处理的，可以写循环语句和判断语句修改格式
````
function ExecuteScript()
{
	document.body.innerHTML = document.body.innerHTML.replace(/class="MathJax_SVG"/g, 'class="MathJax_SVG notranslate"');
	
	document.body.innerHTML = document.body.innerHTML.replace(/class="MathJax CtxtMenu_Attached_0"/g, 'class="MathJax CtxtMenu_Attached_0 notranslate"');
	
	document.body.innerHTML = document.body.innerHTML.replace(/class="MathJax_Preview"/g, 'class="MathJax_Preview notranslate"');
	
	
	document.body.innerHTML = document.body.innerHTML.replace(/<sub>/g, '<sub class="notranslate">');
}
````


power automate流
````
Variables.CreateNewList List=> List_website
Variables.AddItemToList Item: $'''https://link.springer.com/article/10.1007/s10439-022-02982-5''' List: List_website
DISABLE Variables.AddItemToList Item: $'''https://www.sciencedirect.com/science/article/pii/S0022519309005268''' List: List_website
DISABLE Variables.AddItemToList Item: $'''https://www.sciencedirect.com/science/article/pii/S0006899309026092''' List: List_website
DISABLE Variables.AddItemToList Item: $'''https://www.nature.com/articles/s41467-021-27534-8''' List: List_website
DISABLE Variables.AddItemToList Item: $'''https://www.sciencedirect.com/science/article/pii/S2666522021000083''' List: List_website
DISABLE Variables.AddItemToList Item: $'''https://www.sciencedirect.com/science/article/pii/S1053811910013327''' List: List_website
DISABLE Variables.AddItemToList Item: $'''https://www.sciencedirect.com/science/article/pii/S1053811910012139''' List: List_website
DISABLE Variables.AddItemToList Item: $'''https://www.sciencedirect.com/science/article/pii/S0010482519301544''' List: List_website
Variables.AddItemToList Item: $'''https://onlinelibrary.wiley.com/doi/full/10.1111/micc.12687''' List: List_website
DISABLE Variables.AddItemToList Item: 24972 List: List_website_length
DISABLE Variables.AddItemToList Item: 36790 List: List_website_length
DISABLE Variables.AddItemToList Item: 30198 List: List_website_length
DISABLE Variables.AddItemToList Item: 36009 List: List_website_length
LOOP page_index FROM 0 TO List_website.Count - 1 STEP 1
    SET website_name TO List_website[page_index]
    DISABLE SET page_index TO 1
    DISABLE MouseAndKeyboard.SendKeys.FocusAndSendKeys TextToSend: $'''{LControlKey}{NumPad1}''' DelayBetweenKeystrokes: 10 SendTextAsHardwareKeys: False
    WebAutomation.LaunchEdge.AttachToEdgeByUrl TabUrl: website_name AttachTimeout: 5 BrowserInstance=> Browser
    MouseAndKeyboard.SendKeys.FocusAndSendKeys TextToSend: $'''{LControlKey}({NumPad%page_index + 1%})''' DelayBetweenKeystrokes: 10 SendTextAsHardwareKeys: False
    MouseAndKeyboard.SendKeys.FocusAndSendKeys TextToSend: $'''{LControlKey}({Home})''' DelayBetweenKeystrokes: 10 SendTextAsHardwareKeys: False
    DISABLE MouseAndKeyboard.SendKeys.FocusAndSendKeys TextToSend: $'''{LControlKey}({NumPad1})''' DelayBetweenKeystrokes: 10 SendTextAsHardwareKeys: False
    WebAutomation.ExecuteJavascript BrowserInstance: Browser Javascript: $'''function ExecuteScript()
{
    return document.body.scrollHeight
}''' Result=> height
    WebAutomation.ExecuteJavascript BrowserInstance: Browser Javascript: $'''function ExecuteScript()
{
    document.body.innerHTML = document.body.innerHTML.replace(/class=\"MathJax_SVG\"/g, \'class=\"MathJax_SVG notranslate\"\');
    
    document.body.innerHTML = document.body.innerHTML.replace(/class=\"MathJax CtxtMenu_Attached_0\"/g, \'class=\"MathJax CtxtMenu_Attached_0 notranslate\"\');
    
    document.body.innerHTML = document.body.innerHTML.replace(/class=\"MathJax_Preview\"/g, \'class=\"MathJax_Preview notranslate\"\');
    
    
    document.body.innerHTML = document.body.innerHTML.replace(/<sub>/g, \'<sub class=\"notranslate\">\');
}''' Result=> temp
    DISABLE SET height TO List_website_length[page_index]
    WAIT 3
    MouseAndKeyboard.MoveMouse X: 1451 Y: 49 RelativeTo: MouseAndKeyboard.PositionRelativeTo.Screen MovementStyle: MouseAndKeyboard.MovementStyle.Instant
    MouseAndKeyboard.SendMouseClick.Click ClickType: MouseAndKeyboard.MouseClickType.LeftClick MillisecondsDelay: 200
    WAIT 3
    MouseAndKeyboard.MoveMouse X: 1186 Y: 174 RelativeTo: MouseAndKeyboard.PositionRelativeTo.Screen MovementStyle: MouseAndKeyboard.MovementStyle.Instant
    MouseAndKeyboard.SendMouseClick.Click ClickType: MouseAndKeyboard.MouseClickType.LeftClick MillisecondsDelay: 200
    WAIT 3
    MouseAndKeyboard.MoveMouse X: 1196 Y: 525 RelativeTo: MouseAndKeyboard.PositionRelativeTo.Screen MovementStyle: MouseAndKeyboard.MovementStyle.Instant
    MouseAndKeyboard.SendMouseClick.Click ClickType: MouseAndKeyboard.MouseClickType.LeftClick MillisecondsDelay: 200
    WAIT 3
    MouseAndKeyboard.MoveMouse X: 41 Y: 192 RelativeTo: MouseAndKeyboard.PositionRelativeTo.Screen MovementStyle: MouseAndKeyboard.MovementStyle.Instant
    MouseAndKeyboard.SendMouseClick.Click ClickType: MouseAndKeyboard.MouseClickType.RightClick MillisecondsDelay: 200
    WAIT 3
    DISABLE SET tx TO 41
    DISABLE SET ty TO 451
    DISABLE MouseAndKeyboard.MoveMouse X: tx Y: ty RelativeTo: MouseAndKeyboard.PositionRelativeTo.Screen MovementStyle: MouseAndKeyboard.MovementStyle.Instant
    MouseAndKeyboard.MoveMouse X: 41 Y: 441 RelativeTo: MouseAndKeyboard.PositionRelativeTo.Screen MovementStyle: MouseAndKeyboard.MovementStyle.Instant
    MouseAndKeyboard.SendMouseClick.Click ClickType: MouseAndKeyboard.MouseClickType.LeftClick MillisecondsDelay: 200
    WAIT 3
    SET single_height TO 800
    SET number TO $'''ceil(%height% / %single_height%)'''
    Scripting.RunJavascript.RunJavascript JavascriptCode: $'''var a=Math.ceil( %height% / %single_height% )
WScript.Echo(a)''' ScriptOutput=> num
    Text.ToNumber Text: num Number=> num
    SET ith TO 1
    LOOP LoopIndex FROM 1 TO num STEP 1
        DISABLE WebAutomation.ExecuteJavascript BrowserInstance: Browser Javascript: $'''function work()
{
    window.scrollTo(0, %ith%*%single_height%);
}

function ExecuteScript() 
{
    myVar=setTimeout(\"work()\",3000);
}''' Result=> Result
        WebAutomation.ExecuteJavascript BrowserInstance: Browser Javascript: $'''function ExecuteScript() 
{
window.scrollTo(0, %ith%*%single_height%);
}''' Result=> Result
        Variables.IncreaseVariable Value: ith IncrementValue: 1
        WAIT 2.5
    END
    WAIT 3
    MouseAndKeyboard.MoveMouse X: 1589 Y: 50 RelativeTo: MouseAndKeyboard.PositionRelativeTo.Screen MovementStyle: MouseAndKeyboard.MovementStyle.Instant
    MouseAndKeyboard.SendMouseClick.Click ClickType: MouseAndKeyboard.MouseClickType.LeftClick MillisecondsDelay: 500
    WAIT 3
    MouseAndKeyboard.MoveMouse X: 1451 Y: 49 RelativeTo: MouseAndKeyboard.PositionRelativeTo.Screen MovementStyle: MouseAndKeyboard.MovementStyle.Instant
    MouseAndKeyboard.SendMouseClick.Click ClickType: MouseAndKeyboard.MouseClickType.LeftClick MillisecondsDelay: 200
    WAIT 3
    MouseAndKeyboard.MoveMouse X: 1186 Y: 174 RelativeTo: MouseAndKeyboard.PositionRelativeTo.Screen MovementStyle: MouseAndKeyboard.MovementStyle.Instant
    MouseAndKeyboard.SendMouseClick.Click ClickType: MouseAndKeyboard.MouseClickType.LeftClick MillisecondsDelay: 200
    WAIT 3
    MouseAndKeyboard.MoveMouse X: 1196 Y: 525 RelativeTo: MouseAndKeyboard.PositionRelativeTo.Screen MovementStyle: MouseAndKeyboard.MovementStyle.Instant
    MouseAndKeyboard.SendMouseClick.Click ClickType: MouseAndKeyboard.MouseClickType.LeftClick MillisecondsDelay: 200
    WAIT 3
END
````
