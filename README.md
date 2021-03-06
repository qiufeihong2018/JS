# 你不知道的CSS
## 三角形
> 利用transparent透明属性,如果border每个方向上都不透明,则就是个正方形,想使之变成一个有朝向的三角形,只要该朝向的反向不透明其余透明即可.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>triangle</title>
    <style>
        #triangle {
            width: 0;
            height: 0;
            border-top: 300px solid transparent;
            border-left: 300px solid transparent;
            border-right: 300px solid transparent;
            border-bottom: 300px solid #ff0000;
        }
    </style>
</head>
<body>
<div id="triangle"></div>
</body>
</html>

```
![avatar](./css/public/triangle.png)

将三角形补全
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>triangle</title>
    <style>
        #triangle {
            width: 0;
            height: 0;
            border-top: 300px solid #ff0000;
            border-left: 300px solid #ff0000;
            border-right: 300px solid #ff0000;
            border-bottom: 300px solid #ff0000;
        }
    </style>
</head>
<body>
<div id="triangle"></div>
</body>
</html>

```
![avatar](./css/public/square.png)

# 你不知道的JS
## new
```js
// function test(name) {
//     this.name = name
//     console.log(this)
// }
//
// test.prototype.sayName = function () {
//     console.log(this.name)
// }
// const t = new test('qfh')
// t.name
// t.sayName()
// //new出来的实例,可以访问构造函数的属性.
// //new出来的实例,可以访问构造函数原型上的方法,通过new,实例与构造函数通过原型链连接了起来.
// function Test(name) {
//     this.name = name
//     // return 'dfasdfasd'
//     return 231231 || 'dfasdfasdf'
// }
//
// const t = new Test('qfh')
// console.log(t.name)

// 如果构造函数返回原始值,那么这个构造函数毫无意义


// function Test(name) {
//     this.name = name
//     return {ddd: 23123}
// }
//
// const t = new Test('qfh')
// console.log(t.name)
// 如果构造函数返回对象,返回值会正常使用


// new:
// - new操作符会返回一个对象
// - 对象可以访问到挂载在this上的任意属性
// - 对象可以访问到构造函数原型上的属性,所以需要将对象与构造函数链接起来
// - 返回原始值无效,返回对象有效

function create(Con, ...args) {
    let obj = {}
    Object.setPrototypeOf(obj, Con.prototype)
    let result = Con.apply(obj, args)
    return result instanceof Object ? result : obj

}

// - Con构造函数,args被构造函数所使用的参数
// - 创建空对象obj
// - 等同于 obj._proto_=Con.prototype
// - 构造函数绑定对象和其余参数
// - 判断构造函数返回的是否是对象?返回对象:否则就是对象本身

function Test(name) {
    this.name = name
    // return '3123123'
    return {
        aa: 12312,
        ddd:234234
    }
}

const t = create(Test, 'qfh')
console.log(t.ddd)

```

## rest
```js
function f(...args) {
    console.log(args)//[ 1, 2, 3, 123, 434, 54, 65563566 ]
    console.log(args[2] + args[3] + args[4])//560
}

f(1, 2, 3, 123, 434, 54, 65563566)
// f(...args)=f(1, 2, 3, 123, 434, 54, 65563566)
// ...args叫做rest参数

```

## generator
```js
function* greeter() {
    yield 'hi'
    yield 'hi2'
    yield 'hi3'
    yield 'hi4'
}

const greet = greeter()
console.log(greet.next().value)
console.log(greet.next().value)
console.log(greet.next().value)
console.log(greet.next().value)
console.log(greet.next().value)
// hi
// hi2
// hi3
// hi4
// undefined
// * yield 那就是generator,生成器函数指下一次next()生成的value,可以是有限的(最后是undefined),也可以是无限的.

```

## promise
```js
const myPromise = new Promise(function (resolve, reject) {
    if (Math.random() < 0.9) {
        return resolve('aaa')
    }
    return reject('bbb')
})

myPromise.then(res => {
    console.log('aaa', res)
}).catch(err => {
    console.log('bbb', err)
})
// promise解决回调地域!将异步逻辑包装在promise中,使用'then'处理成功的,'catch'处理异常的.


```

## prototype
> 但是为什么实例对象里的变量的值却是A呢
```js
// B.prototype是一个实例对象,那么c这个实例对象可以访问到A里面的name属性,如果name有值那么就赋值,否则就输出默认值.
function A(name) {
    this.name = name || 'hhh'
}

function B() {
}

B.prototype = new A()
const c = new B()
console.log(c)//A {}
console.log(c.name)//hhh


// B.prototype是一个函数对象,那么c这个实例对象不能访问到A的内部,看打印出来的c,可以看出是一个函数对象,那么就只能访问函数对象的name属性,即为他的函数名
function A(name) {
    this.name = name || 'hhh'
}

function B() {
}

B.prototype = A
const c = new B()
console.log(c)//Function {}
console.log(c.name)//A
函数对象都有隐藏的name属性,且该属性保存的是该函数名称的字符串.
例如:
```js
function b(){}
console.log(b.name)//b
```

> [JavaScript中函数的name属性](https://blog.csdn.net/goo_oooo/article/details/79242234)

## sort

>  sortFun()是一个函数,是sort排序的依据,<=0则不动,大于0则交换顺序

```js
let arr = [10, 145, 2, 455, 8, -456, 1536, 88, -54,]
const sortFun = (first, second) => first - second
arr.sort(sortFun)
console.log(arr)//[ -456, -54, 2, 8, 10, 88, 145, 455, 1536 ]

```

## parseInt
- 第一个参数s是要转整数的数
- 第二个参数radix是大于2小于36的,是基数，
- 第三个参数没有任何用途
```js
console.log(parseInt(1212, 10))// 1212

console.log(parseInt(123.3123, 10))// 123

console.log(parseInt(1111011, 2))// 123

console.log(parseInt(111, 38))// NaN

console.log(parseInt(111, 3))// 13

console.log(parseInt('1', 2, [1]))// 1

console.log(parseInt('1', 0, ['1', '2', '3']))// 1  0就相当于10进制

console.log(parseInt('2', 1, ['1', '2', '3']))// NaN   没有1进制

console.log(parseInt('3', 2, ['1', '2', '3']))// NaN   2进制是没有3的


```

## forEach

> 数组遍历forEach方法

- 普通函数和箭头函数的区别：普通函数this作用域在这个函数内，但是箭头函数的this作用域突破了这个函数在全局作用域。
- 方法中的第二个可以不填，他表示函数内部的this
```js
// [12, 4312, 54343, 5345].forEach(function (x) {
//     console.log(this)
// })
// // Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}
// // VM1394:2 Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}
// // VM1394:2 Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}alert: ƒ alert()applicationCache: ApplicationCache {status: 0, oncached: null, onchecking: null, ondownloading: null, onerror: null, …}atob: ƒ atob()blur: ƒ ()btoa: ƒ btoa()caches: CacheStorage {}cancelAnimationFrame: ƒ cancelAnimationFrame()cancelIdleCallback: ƒ cancelIdleCallback()captureEvents: ƒ captureEvents()chrome: {loadTimes: ƒ, csi: ƒ, …}clearInterval: ƒ clearInterval()clearTimeout: ƒ clearTimeout()clientInformation: Navigator {vendorSub: "", productSub: "20030107", vendor: "Google Inc.", maxTouchPoints: 0, hardwareConcurrency: 4, …}close: ƒ ()closed: falseconfirm: ƒ confirm()createImageBitmap: ƒ createImageBitmap()crypto: Crypto {subtle: SubtleCrypto}customElements: CustomElementRegistry {}defaultStatus: ""defaultstatus: ""devicePixelRatio: 1.5document: documentexternal: External {}fetch: ƒ fetch()find: ƒ find()focus: ƒ ()frameElement: nullframes: Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}ga: ƒ (a)gaData: {UA-61022745-2: {…}}gaGlobal: {hid: 1559868783}gaplugins: {Linker: ƒ}getComputedStyle: ƒ getComputedStyle()getSelection: ƒ getSelection()goldBridge: {fetch: ƒ, rawFetch: ƒ, messenger: e, storage: {…}, tab: {…}, …}history: History {length: 1, scrollRestoration: "auto", state: null}indexedDB: IDBFactory {}innerHeight: 635innerWidth: 923isSecureContext: truelength: 0loadCode: ƒ (code)localStorage: Storage {length: 0}location: Location {replace: ƒ, assign: ƒ, href: "chrome-extension://lecdifefmmfjnjjinhaennhdlmcaeeeb/main.html", ancestorOrigins: DOMStringList, origin: "chrome-extension://lecdifefmmfjnjjinhaennhdlmcaeeeb", …}locationbar: BarProp {visible: true}matchMedia: ƒ matchMedia()menubar: BarProp {visible: true}moveBy: ƒ moveBy()moveTo: ƒ moveTo()name: ""navigator: Navigator {vendorSub: "", productSub: "20030107", vendor: "Google Inc.", maxTouchPoints: 0, hardwareConcurrency: 4, …}onabort: nullonafterprint: nullonanimationend: nullonanimationiteration: nullonanimationstart: nullonappinstalled: nullonauxclick: nullonbeforeinstallprompt: nullonbeforeprint: nullonbeforeunload: nullonblur: nulloncancel: nulloncanplay: nulloncanplaythrough: nullonchange: nullonclick: nullonclose: nulloncontextmenu: nulloncuechange: nullondblclick: nullondevicemotion: nullondeviceorientation: nullondeviceorientationabsolute: nullondrag: nullondragend: nullondragenter: nullondragleave: nullondragover: nullondragstart: nullondrop: nullondurationchange: nullonemptied: nullonended: nullonerror: nullonfocus: nullongotpointercapture: nullonhashchange: nulloninput: nulloninvalid: nullonkeydown: nullonkeypress: nullonkeyup: nullonlanguagechange: nullonload: nullonloadeddata: nullonloadedmetadata: nullonloadstart: nullonlostpointercapture: nullonmessage: nullonmessageerror: nullonmousedown: nullonmouseenter: nullonmouseleave: nullonmousemove: nullonmouseout: nullonmouseover: nullonmouseup: nullonmousewheel: nullonoffline: nullononline: nullonpagehide: nullonpageshow: nullonpause: nullonplay: nullonplaying: nullonpointercancel: nullonpointerdown: nullonpointerenter: nullonpointerleave: nullonpointermove: nullonpointerout: nullonpointerover: nullonpointerup: nullonpopstate: nullonprogress: nullonratechange: nullonrejectionhandled: nullonreset: nullonresize: nullonscroll: nullonsearch: nullonseeked: nullonseeking: nullonselect: nullonselectionchange: nullonselectstart: nullonstalled: nullonstorage: nullonsubmit: nullonsuspend: nullontimeupdate: nullontoggle: nullontransitionend: nullonunhandledrejection: nullonunload: nullonvolumechange: nullonwaiting: nullonwebkitanimationend: nullonwebkitanimationiteration: nullonwebkitanimationstart: nullonwebkittransitionend: nullonwheel: nullopen: ƒ open()openDatabase: ƒ ()opener: nullorigin: "chrome-extension://lecdifefmmfjnjjinhaennhdlmcaeeeb"outerHeight: 1056outerWidth: 1855pageXOffset: 0pageYOffset: 0parent: Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}performance: Performance {timeOrigin: 1553327265618.618, onresourcetimingbufferfull: null, memory: MemoryInfo, navigation: PerformanceNavigation, timing: PerformanceTiming}personalbar: BarProp {visible: true}postMessage: ƒ ()print: ƒ print()prompt: ƒ prompt()queueMicrotask: ƒ queueMicrotask()releaseEvents: ƒ releaseEvents()requestAnimationFrame: ƒ requestAnimationFrame()requestIdleCallback: ƒ requestIdleCallback()resizeBy: ƒ resizeBy()resizeTo: ƒ resizeTo()screen: Screen {availWidth: 1920, availHeight: 1056, width: 1920, height: 1080, colorDepth: 24, …}screenLeft: 1985screenTop: 24screenX: 1985screenY: 24scroll: ƒ scroll()scrollBy: ƒ scrollBy()scrollTo: ƒ scrollTo()scrollX: 0scrollY: 0scrollbars: BarProp {visible: true}self: Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}sessionStorage: Storage {length: 0}setInterval: ƒ setInterval()setTimeout: ƒ setTimeout()silence: ƒ ()speechSynthesis: SpeechSynthesis {pending: false, speaking: false, paused: false, onvoiceschanged: null}status: ""statusbar: BarProp {visible: true}stop: ƒ stop()styleMedia: StyleMedia {type: "screen"}toolbar: BarProp {visible: true}top: Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}visualViewport: VisualViewport {offsetLeft: 0, offsetTop: 0, pageLeft: 0, pageTop: 0, width: 923.3333129882812, …}webkitCancelAnimationFrame: ƒ webkitCancelAnimationFrame()webkitRequestAnimationFrame: ƒ webkitRequestAnimationFrame()webkitRequestFileSystem: ƒ ()webkitResolveLocalFileSystemURL: ƒ ()webkitStorageInfo: DeprecatedStorageInfo {}window: Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}__core-js_shared__: {wks: {…}, symbol-registry: {…}, symbols: {…}}Infinity: InfinityAbortController: ƒ AbortController()AbortSignal: ƒ AbortSignal()AbsoluteOrientationSensor: ƒ AbsoluteOrientationSensor()Accelerometer: ƒ Accelerometer()AnalyserNode: ƒ AnalyserNode()AnimationEvent: ƒ AnimationEvent()AppView: class extendsApplicationCache: ƒ ApplicationCache()ApplicationCacheErrorEvent: ƒ ApplicationCacheErrorEvent()Array: ƒ Array()ArrayBuffer: ƒ ArrayBuffer()Atomics: Atomics {load: ƒ, store: ƒ, add: ƒ, sub: ƒ, and: ƒ, …}Attr: ƒ Attr()Audio: ƒ Audio()AudioBuffer: ƒ AudioBuffer()AudioBufferSourceNode: ƒ AudioBufferSourceNode()AudioContext: ƒ AudioContext()AudioDestinationNode: ƒ AudioDestinationNode()AudioListener: ƒ AudioListener()AudioNode: ƒ AudioNode()AudioParam: ƒ AudioParam()AudioParamMap: ƒ AudioParamMap()AudioProcessingEvent: ƒ AudioProcessingEvent()AudioScheduledSourceNode: ƒ AudioScheduledSourceNode()AudioWorklet: ƒ AudioWorklet()AudioWorkletNode: ƒ AudioWorkletNode()AuthenticatorAssertionResponse: ƒ AuthenticatorAssertionResponse()AuthenticatorAttestationResponse: ƒ AuthenticatorAttestationResponse()AuthenticatorResponse: ƒ AuthenticatorResponse()BarProp: ƒ BarProp()BaseAudioContext: ƒ BaseAudioContext()BatteryManager: ƒ BatteryManager()BeforeInstallPromptEvent: ƒ BeforeInstallPromptEvent()BeforeUnloadEvent: ƒ BeforeUnloadEvent()BigInt: ƒ BigInt()BigInt64Array: ƒ BigInt64Array()BigUint64Array: ƒ BigUint64Array()BiquadFilterNode: ƒ BiquadFilterNode()Blob: ƒ Blob()BlobEvent: ƒ BlobEvent()Boolean: ƒ Boolean()BroadcastChannel: ƒ BroadcastChannel()ByteLengthQueuingStrategy: ƒ ByteLengthQueuingStrategy()CDATASection: ƒ CDATASection()CSS: ƒ CSS()CSSConditionRule: ƒ CSSConditionRule()CSSFontFaceRule: ƒ CSSFontFaceRule()CSSGroupingRule: ƒ CSSGroupingRule()CSSImageValue: ƒ CSSImageValue()CSSImportRule: ƒ CSSImportRule()CSSKeyframeRule: ƒ CSSKeyframeRule()CSSKeyframesRule: ƒ CSSKeyframesRule()CSSKeywordValue: ƒ CSSKeywordValue()CSSMathInvert: ƒ CSSMathInvert()CSSMathMax: ƒ CSSMathMax()CSSMathMin: ƒ CSSMathMin()CSSMathNegate: ƒ CSSMathNegate()CSSMathProduct: ƒ CSSMathProduct()CSSMathSum: ƒ CSSMathSum()CSSMathValue: ƒ CSSMathValue()CSSMatrixComponent: ƒ CSSMatrixComponent()CSSMediaRule: ƒ CSSMediaRule()CSSNamespaceRule: ƒ CSSNamespaceRule()CSSNumericArray: ƒ CSSNumericArray()CSSNumericValue: ƒ CSSNumericValue()CSSPageRule: ƒ CSSPageRule()CSSPerspective: ƒ CSSPerspective()CSSPositionValue: ƒ CSSPositionValue()CSSRotate: ƒ CSSRotate()CSSRule: ƒ CSSRule()CSSRuleList: ƒ CSSRuleList()CSSScale: ƒ CSSScale()CSSSkew: ƒ CSSSkew()CSSSkewX: ƒ CSSSkewX()CSSSkewY: ƒ CSSSkewY()CSSStyleDeclaration: ƒ CSSStyleDeclaration()CSSStyleRule: ƒ CSSStyleRule()CSSStyleSheet: ƒ CSSStyleSheet()CSSStyleValue: ƒ CSSStyleValue()CSSSupportsRule: ƒ CSSSupportsRule()CSSTransformComponent: ƒ CSSTransformComponent()CSSTransformValue: ƒ CSSTransformValue()CSSTranslate: ƒ CSSTranslate()CSSUnitValue: ƒ CSSUnitValue()CSSUnparsedValue: ƒ CSSUnparsedValue()CSSVariableReferenceValue: ƒ CSSVariableReferenceValue()Cache: ƒ Cache()CacheStorage: ƒ CacheStorage()CanvasCaptureMediaStreamTrack: ƒ CanvasCaptureMediaStreamTrack()CanvasGradient: ƒ CanvasGradient()CanvasPattern: ƒ CanvasPattern()CanvasRenderingContext2D: ƒ CanvasRenderingContext2D()ChannelMergerNode: ƒ ChannelMergerNode()ChannelSplitterNode: ƒ ChannelSplitterNode()CharacterData: ƒ CharacterData()Clipboard: ƒ Clipboard()ClipboardEvent: ƒ ClipboardEvent()CloseEvent: ƒ CloseEvent()Comment: ƒ Comment()CompositionEvent: ƒ CompositionEvent()ConstantSourceNode: ƒ ConstantSourceNode()ConvolverNode: ƒ ConvolverNode()CountQueuingStrategy: ƒ CountQueuingStrategy()Credential: ƒ Credential()CredentialsContainer: ƒ CredentialsContainer()Crypto: ƒ Crypto()CryptoKey: ƒ CryptoKey()CustomElementRegistry: ƒ CustomElementRegistry()CustomEvent: ƒ CustomEvent()DOMError: ƒ DOMError()DOMException: ƒ DOMException()DOMImplementation: ƒ DOMImplementation()DOMMatrix: ƒ DOMMatrix()DOMMatrixReadOnly: ƒ DOMMatrixReadOnly()DOMParser: ƒ DOMParser()DOMPoint: ƒ DOMPoint()DOMPointReadOnly: ƒ DOMPointReadOnly()DOMQuad: ƒ DOMQuad()DOMRect: ƒ DOMRect()DOMRectList: ƒ DOMRectList()DOMRectReadOnly: ƒ DOMRectReadOnly()DOMStringList: ƒ DOMStringList()DOMStringMap: ƒ DOMStringMap()DOMTokenList: ƒ DOMTokenList()DataTransfer: ƒ DataTransfer()DataTransferItem: ƒ DataTransferItem()DataTransferItemList: ƒ DataTransferItemList()DataView: ƒ DataView()Date: ƒ Date()DelayNode: ƒ DelayNode()DeviceMotionEvent: ƒ DeviceMotionEvent()DeviceOrientationEvent: ƒ DeviceOrientationEvent()Document: ƒ Document()DocumentFragment: ƒ DocumentFragment()DocumentType: ƒ DocumentType()DragEvent: ƒ DragEvent()DynamicsCompressorNode: ƒ DynamicsCompressorNode()Element: ƒ Element()EnterPictureInPictureEvent: ƒ EnterPictureInPictureEvent()Error: ƒ Error()ErrorEvent: ƒ ErrorEvent()EvalError: ƒ EvalError()Event: ƒ Event()EventSource: ƒ EventSource()EventTarget: ƒ EventTarget()ExtensionOptions: class extendsExtensionView: class extendsFederatedCredential: ƒ FederatedCredential()File: ƒ File()FileList: ƒ FileList()FileReader: ƒ FileReader()Float32Array: ƒ Float32Array()Float64Array: ƒ Float64Array()FocusEvent: ƒ FocusEvent()FontFace: ƒ FontFace()FontFaceSetLoadEvent: ƒ FontFaceSetLoadEvent()FormData: ƒ FormData()Function: ƒ Function()GainNode: ƒ GainNode()Gamepad: ƒ Gamepad()GamepadButton: ƒ GamepadButton()GamepadEvent: ƒ GamepadEvent()GamepadHapticActuator: ƒ GamepadHapticActuator()Gyroscope: ƒ Gyroscope()HTMLAllCollection: ƒ HTMLAllCollection()HTMLAnchorElement: ƒ HTMLAnchorElement()HTMLAreaElement: ƒ HTMLAreaElement()HTMLAudioElement: ƒ HTMLAudioElement()HTMLBRElement: ƒ HTMLBRElement()HTMLBaseElement: ƒ HTMLBaseElement()HTMLBodyElement: ƒ HTMLBodyElement()HTMLButtonElement: ƒ HTMLButtonElement()HTMLCanvasElement: ƒ HTMLCanvasElement()HTMLCollection: ƒ HTMLCollection()HTMLContentElement: ƒ HTMLContentElement()HTMLDListElement: ƒ HTMLDListElement()HTMLDataElement: ƒ HTMLDataElement()HTMLDataListElement: ƒ HTMLDataListElement()HTMLDetailsElement: ƒ HTMLDetailsElement()HTMLDialogElement: ƒ HTMLDialogElement()HTMLDirectoryElement: ƒ HTMLDirectoryElement()HTMLDivElement: ƒ HTMLDivElement()HTMLDocument: ƒ HTMLDocument()HTMLElement: ƒ HTMLElement()HTMLEmbedElement: ƒ HTMLEmbedElement()HTMLFieldSetElement: ƒ HTMLFieldSetElement()HTMLFontElement: ƒ HTMLFontElement()HTMLFormControlsCollection: ƒ HTMLFormControlsCollection()HTMLFormElement: ƒ HTMLFormElement()HTMLFrameElement: ƒ HTMLFrameElement()HTMLFrameSetElement: ƒ HTMLFrameSetElement()HTMLHRElement: ƒ HTMLHRElement()HTMLHeadElement: ƒ HTMLHeadElement()HTMLHeadingElement: ƒ HTMLHeadingElement()HTMLHtmlElement: ƒ HTMLHtmlElement()HTMLIFrameElement: ƒ HTMLIFrameElement()HTMLImageElement: ƒ HTMLImageElement()HTMLInputElement: ƒ HTMLInputElement()HTMLLIElement: ƒ HTMLLIElement()HTMLLabelElement: ƒ HTMLLabelElement()HTMLLegendElement: ƒ HTMLLegendElement()HTMLLinkElement: ƒ HTMLLinkElement()HTMLMapElement: ƒ HTMLMapElement()HTMLMarqueeElement: ƒ HTMLMarqueeElement()HTMLMediaElement: ƒ HTMLMediaElement()HTMLMenuElement: ƒ HTMLMenuElement()HTMLMetaElement: ƒ HTMLMetaElement()HTMLMeterElement: ƒ HTMLMeterElement()HTMLModElement: ƒ HTMLModElement()HTMLOListElement: ƒ HTMLOListElement()HTMLObjectElement: ƒ HTMLObjectElement()HTMLOptGroupElement: ƒ HTMLOptGroupElement()HTMLOptionElement: ƒ HTMLOptionElement()HTMLOptionsCollection: ƒ HTMLOptionsCollection()HTMLOutputElement: ƒ HTMLOutputElement()HTMLParagraphElement: ƒ HTMLParagraphElement()HTMLParamElement: ƒ HTMLParamElement()HTMLPictureElement: ƒ HTMLPictureElement()HTMLPreElement: ƒ HTMLPreElement()HTMLProgressElement: ƒ HTMLProgressElement()HTMLQuoteElement: ƒ HTMLQuoteElement()HTMLScriptElement: ƒ HTMLScriptElement()HTMLSelectElement: ƒ HTMLSelectElement()HTMLShadowElement: ƒ HTMLShadowElement()HTMLSlotElement: ƒ HTMLSlotElement()HTMLSourceElement: ƒ HTMLSourceElement()HTMLSpanElement: ƒ HTMLSpanElement()HTMLStyleElement: ƒ HTMLStyleElement()HTMLTableCaptionElement: ƒ HTMLTableCaptionElement()HTMLTableCellElement: ƒ HTMLTableCellElement()HTMLTableColElement: ƒ HTMLTableColElement()HTMLTableElement: ƒ HTMLTableElement()HTMLTableRowElement: ƒ HTMLTableRowElement()HTMLTableSectionElement: ƒ HTMLTableSectionElement()HTMLTemplateElement: ƒ HTMLTemplateElement()HTMLTextAreaElement: ƒ HTMLTextAreaElement()HTMLTimeElement: ƒ HTMLTimeElement()HTMLTitleElement: ƒ HTMLTitleElement()HTMLTrackElement: ƒ HTMLTrackElement()HTMLUListElement: ƒ HTMLUListElement()HTMLUnknownElement: ƒ HTMLUnknownElement()HTMLVideoElement: ƒ HTMLVideoElement()HashChangeEvent: ƒ HashChangeEvent()Headers: ƒ Headers()History: ƒ History()IDBCursor: ƒ IDBCursor()IDBCursorWithValue: ƒ IDBCursorWithValue()IDBDatabase: ƒ IDBDatabase()IDBFactory: ƒ IDBFactory()IDBIndex: ƒ IDBIndex()IDBKeyRange: ƒ IDBKeyRange()IDBObjectStore: ƒ IDBObjectStore()IDBOpenDBRequest: ƒ IDBOpenDBRequest()IDBRequest: ƒ IDBRequest()IDBTransaction: ƒ IDBTransaction()IDBVersionChangeEvent: ƒ IDBVersionChangeEvent()IIRFilterNode: ƒ IIRFilterNode()IdleDeadline: ƒ IdleDeadline()Image: ƒ Image()ImageBitmap: ƒ ImageBitmap()ImageBitmapRenderingContext: ƒ ImageBitmapRenderingContext()ImageCapture: ƒ ImageCapture()ImageData: ƒ ImageData()InputDeviceCapabilities: ƒ InputDeviceCapabilities()InputDeviceInfo: ƒ InputDeviceInfo()InputEvent: ƒ InputEvent()Int8Array: ƒ Int8Array()Int16Array: ƒ Int16Array()Int32Array: ƒ Int32Array()IntersectionObserver: ƒ IntersectionObserver()IntersectionObserverEntry: ƒ IntersectionObserverEntry()Intl: {getCanonicalLocales: ƒ, DateTimeFormat: ƒ, NumberFormat: ƒ, Collator: ƒ, v8BreakIterator: ƒ, …}JSON: JSON {parse: ƒ, stringify: ƒ, Symbol(Symbol.toStringTag): "JSON"}Keyboard: ƒ Keyboard()KeyboardEvent: ƒ KeyboardEvent()KeyboardLayoutMap: ƒ KeyboardLayoutMap()LinearAccelerationSensor: ƒ LinearAccelerationSensor()Location: ƒ Location()Lock: ƒ Lock()LockManager: ƒ LockManager()MIDIAccess: ƒ MIDIAccess()MIDIConnectionEvent: ƒ MIDIConnectionEvent()MIDIInput: ƒ MIDIInput()MIDIInputMap: ƒ MIDIInputMap()MIDIMessageEvent: ƒ MIDIMessageEvent()MIDIOutput: ƒ MIDIOutput()MIDIOutputMap: ƒ MIDIOutputMap()MIDIPort: ƒ MIDIPort()Map: ƒ Map()Math: Math {abs: ƒ, acos: ƒ, acosh: ƒ, asin: ƒ, asinh: ƒ, …}MediaCapabilities: ƒ MediaCapabilities()MediaDeviceInfo: ƒ MediaDeviceInfo()MediaDevices: ƒ MediaDevices()MediaElementAudioSourceNode: ƒ MediaElementAudioSourceNode()MediaEncryptedEvent: ƒ MediaEncryptedEvent()MediaError: ƒ MediaError()MediaKeyMessageEvent: ƒ MediaKeyMessageEvent()MediaKeySession: ƒ MediaKeySession()MediaKeyStatusMap: ƒ MediaKeyStatusMap()MediaKeySystemAccess: ƒ MediaKeySystemAccess()MediaKeys: ƒ MediaKeys()MediaList: ƒ MediaList()MediaQueryList: ƒ MediaQueryList()MediaQueryListEvent: ƒ MediaQueryListEvent()MediaRecorder: ƒ MediaRecorder()MediaSettingsRange: ƒ MediaSettingsRange()MediaSource: ƒ MediaSource()MediaStream: ƒ MediaStream()MediaStreamAudioDestinationNode: ƒ MediaStreamAudioDestinationNode()MediaStreamAudioSourceNode: ƒ MediaStreamAudioSourceNode()MediaStreamEvent: ƒ MediaStreamEvent()MediaStreamTrack: ƒ MediaStreamTrack()MediaStreamTrackEvent: ƒ MediaStreamTrackEvent()MessageChannel: ƒ MessageChannel()MessageEvent: ƒ MessageEvent()MessagePort: ƒ MessagePort()MimeType: ƒ MimeType()MimeTypeArray: ƒ MimeTypeArray()MouseEvent: ƒ MouseEvent()MutationEvent: ƒ MutationEvent()MutationObserver: ƒ MutationObserver()MutationRecord: ƒ MutationRecord()NaN: NaNNamedNodeMap: ƒ NamedNodeMap()NavigationPreloadManager: ƒ NavigationPreloadManager()Navigator: ƒ Navigator()NetworkInformation: ƒ NetworkInformation()Node: ƒ Node()NodeFilter: ƒ NodeFilter()NodeIterator: ƒ NodeIterator()NodeList: ƒ NodeList()Notification: ƒ Notification()Number: ƒ Number()Object: ƒ Object()OfflineAudioCompletionEvent: ƒ OfflineAudioCompletionEvent()OfflineAudioContext: ƒ OfflineAudioContext()OffscreenCanvas: ƒ OffscreenCanvas()OffscreenCanvasRenderingContext2D: ƒ OffscreenCanvasRenderingContext2D()Option: ƒ Option()OrientationSensor: ƒ OrientationSensor()OscillatorNode: ƒ OscillatorNode()OverconstrainedError: ƒ OverconstrainedError()PageTransitionEvent: ƒ PageTransitionEvent()PannerNode: ƒ PannerNode()PasswordCredential: ƒ PasswordCredential()Path2D: ƒ Path2D()PaymentAddress: ƒ PaymentAddress()PaymentInstruments: ƒ PaymentInstruments()PaymentManager: ƒ PaymentManager()PaymentRequest: ƒ PaymentRequest()PaymentRequestUpdateEvent: ƒ PaymentRequestUpdateEvent()PaymentResponse: ƒ PaymentResponse()Performance: ƒ Performance()PerformanceEntry: ƒ PerformanceEntry()PerformanceLongTaskTiming: ƒ PerformanceLongTaskTiming()PerformanceMark: ƒ PerformanceMark()PerformanceMeasure: ƒ PerformanceMeasure()PerformanceNavigation: ƒ PerformanceNavigation()PerformanceNavigationTiming: ƒ PerformanceNavigationTiming()PerformanceObserver: ƒ PerformanceObserver()PerformanceObserverEntryList: ƒ PerformanceObserverEntryList()PerformancePaintTiming: ƒ PerformancePaintTiming()PerformanceResourceTiming: ƒ PerformanceResourceTiming()PerformanceServerTiming: ƒ PerformanceServerTiming()PerformanceTiming: ƒ PerformanceTiming()PeriodicWave: ƒ PeriodicWave()PermissionStatus: ƒ PermissionStatus()Permissions: ƒ Permissions()PhotoCapabilities: ƒ PhotoCapabilities()PictureInPictureWindow: ƒ PictureInPictureWindow()Plugin: ƒ Plugin()PluginArray: ƒ PluginArray()PointerEvent: ƒ PointerEvent()PopStateEvent: ƒ PopStateEvent()Presentation: ƒ Presentation()PresentationAvailability: ƒ PresentationAvailability()PresentationConnection: ƒ PresentationConnection()PresentationConnectionAvailableEvent: ƒ PresentationConnectionAvailableEvent()PresentationConnectionCloseEvent: ƒ PresentationConnectionCloseEvent()PresentationConnectionList: ƒ PresentationConnectionList()PresentationReceiver: ƒ PresentationReceiver()PresentationRequest: ƒ PresentationRequest()ProcessingInstruction: ƒ ProcessingInstruction()ProgressEvent: ƒ ProgressEvent()Promise: ƒ Promise()PromiseRejectionEvent: ƒ PromiseRejectionEvent()Proxy: ƒ Proxy()PublicKeyCredential: ƒ PublicKeyCredential()PushManager: ƒ PushManager()PushSubscription: ƒ PushSubscription()PushSubscriptionOptions: ƒ PushSubscriptionOptions()RTCCertificate: ƒ RTCCertificate()RTCDTMFSender: ƒ RTCDTMFSender()RTCDTMFToneChangeEvent: ƒ RTCDTMFToneChangeEvent()RTCDataChannel: ƒ RTCDataChannel()RTCDataChannelEvent: ƒ RTCDataChannelEvent()RTCIceCandidate: ƒ RTCIceCandidate()RTCPeerConnection: ƒ RTCPeerConnection()RTCPeerConnectionIceEvent: ƒ RTCPeerConnectionIceEvent()RTCRtpContributingSource: ƒ RTCRtpContributingSource()RTCRtpReceiver: ƒ RTCRtpReceiver()RTCRtpSender: ƒ RTCRtpSender()RTCRtpTransceiver: ƒ RTCRtpTransceiver()RTCSessionDescription: ƒ RTCSessionDescription()RTCStatsReport: ƒ RTCStatsReport()RTCTrackEvent: ƒ RTCTrackEvent()RadioNodeList: ƒ RadioNodeList()Range: ƒ Range()RangeError: ƒ RangeError()ReadableStream: ƒ ReadableStream()ReferenceError: ƒ ReferenceError()Reflect: {defineProperty: ƒ, deleteProperty: ƒ, apply: ƒ, construct: ƒ, get: ƒ, …}RegExp: ƒ RegExp()RelativeOrientationSensor: ƒ RelativeOrientationSensor()RemotePlayback: ƒ RemotePlayback()ReportingObserver: ƒ ReportingObserver()Request: ƒ Request()ResizeObserver: ƒ ResizeObserver()ResizeObserverEntry: ƒ ResizeObserverEntry()Response: ƒ Response()SVGAElement: ƒ SVGAElement()SVGAngle: ƒ SVGAngle()SVGAnimateElement: ƒ SVGAnimateElement()SVGAnimateMotionElement: ƒ SVGAnimateMotionElement()SVGAnimateTransformElement: ƒ SVGAnimateTransformElement()SVGAnimatedAngle: ƒ SVGAnimatedAngle()SVGAnimatedBoolean: ƒ SVGAnimatedBoolean()SVGAnimatedEnumeration: ƒ SVGAnimatedEnumeration()SVGAnimatedInteger: ƒ SVGAnimatedInteger()SVGAnimatedLength: ƒ SVGAnimatedLength()SVGAnimatedLengthList: ƒ SVGAnimatedLengthList()SVGAnimatedNumber: ƒ SVGAnimatedNumber()SVGAnimatedNumberList: ƒ SVGAnimatedNumberList()SVGAnimatedPreserveAspectRatio: ƒ SVGAnimatedPreserveAspectRatio()SVGAnimatedRect: ƒ SVGAnimatedRect()SVGAnimatedString: ƒ SVGAnimatedString()SVGAnimatedTransformList: ƒ SVGAnimatedTransformList()SVGAnimationElement: ƒ SVGAnimationElement()SVGCircleElement: ƒ SVGCircleElement()SVGClipPathElement: ƒ SVGClipPathElement()SVGComponentTransferFunctionElement: ƒ SVGComponentTransferFunctionElement()SVGDefsElement: ƒ SVGDefsElement()SVGDescElement: ƒ SVGDescElement()SVGDiscardElement: ƒ SVGDiscardElement()SVGElement: ƒ SVGElement()SVGEllipseElement: ƒ SVGEllipseElement()SVGFEBlendElement: ƒ SVGFEBlendElement()SVGFEColorMatrixElement: ƒ SVGFEColorMatrixElement()SVGFEComponentTransferElement: ƒ SVGFEComponentTransferElement()SVGFECompositeElement: ƒ SVGFECompositeElement()SVGFEConvolveMatrixElement: ƒ SVGFEConvolveMatrixElement()SVGFEDiffuseLightingElement: ƒ SVGFEDiffuseLightingElement()SVGFEDisplacementMapElement: ƒ SVGFEDisplacementMapElement()SVGFEDistantLightElement: ƒ SVGFEDistantLightElement()SVGFEDropShadowElement: ƒ SVGFEDropShadowElement()SVGFEFloodElement: ƒ SVGFEFloodElement()SVGFEFuncAElement: ƒ SVGFEFuncAElement()SVGFEFuncBElement: ƒ SVGFEFuncBElement()SVGFEFuncGElement: ƒ SVGFEFuncGElement()SVGFEFuncRElement: ƒ SVGFEFuncRElement()SVGFEGaussianBlurElement: ƒ SVGFEGaussianBlurElement()SVGFEImageElement: ƒ SVGFEImageElement()SVGFEMergeElement: ƒ SVGFEMergeElement()SVGFEMergeNodeElement: ƒ SVGFEMergeNodeElement()SVGFEMorphologyElement: ƒ SVGFEMorphologyElement()SVGFEOffsetElement: ƒ SVGFEOffsetElement()SVGFEPointLightElement: ƒ SVGFEPointLightElement()SVGFESpecularLightingElement: ƒ SVGFESpecularLightingElement()SVGFESpotLightElement: ƒ SVGFESpotLightElement()SVGFETileElement: ƒ SVGFETileElement()SVGFETurbulenceElement: ƒ SVGFETurbulenceElement()SVGFilterElement: ƒ SVGFilterElement()SVGForeignObjectElement: ƒ SVGForeignObjectElement()SVGGElement: ƒ SVGGElement()SVGGeometryElement: ƒ SVGGeometryElement()SVGGradientElement: ƒ SVGGradientElement()SVGGraphicsElement: ƒ SVGGraphicsElement()SVGImageElement: ƒ SVGImageElement()SVGLength: ƒ SVGLength()SVGLengthList: ƒ SVGLengthList()SVGLineElement: ƒ SVGLineElement()SVGLinearGradientElement: ƒ SVGLinearGradientElement()SVGMPathElement: ƒ SVGMPathElement()SVGMarkerElement: ƒ SVGMarkerElement()SVGMaskElement: ƒ SVGMaskElement()SVGMatrix: ƒ SVGMatrix()SVGMetadataElement: ƒ SVGMetadataElement()SVGNumber: ƒ SVGNumber()SVGNumberList: ƒ SVGNumberList()SVGPathElement: ƒ SVGPathElement()SVGPatternElement: ƒ SVGPatternElement()SVGPoint: ƒ SVGPoint()SVGPointList: ƒ SVGPointList()SVGPolygonElement: ƒ SVGPolygonElement()SVGPolylineElement: ƒ SVGPolylineElement()SVGPreserveAspectRatio: ƒ SVGPreserveAspectRatio()SVGRadialGradientElement: ƒ SVGRadialGradientElement()SVGRect: ƒ SVGRect()SVGRectElement: ƒ SVGRectElement()SVGSVGElement: ƒ SVGSVGElement()SVGScriptElement: ƒ SVGScriptElement()SVGSetElement: ƒ SVGSetElement()SVGStopElement: ƒ SVGStopElement()SVGStringList: ƒ SVGStringList()SVGStyleElement: ƒ SVGStyleElement()SVGSwitchElement: ƒ SVGSwitchElement()SVGSymbolElement: ƒ SVGSymbolElement()SVGTSpanElement: ƒ SVGTSpanElement()SVGTextContentElement: ƒ SVGTextContentElement()SVGTextElement: ƒ SVGTextElement()SVGTextPathElement: ƒ SVGTextPathElement()SVGTextPositioningElement: ƒ SVGTextPositioningElement()SVGTitleElement: ƒ SVGTitleElement()SVGTransform: ƒ SVGTransform()SVGTransformList: ƒ SVGTransformList()SVGUnitTypes: ƒ SVGUnitTypes()SVGUseElement: ƒ SVGUseElement()SVGViewElement: ƒ SVGViewElement()Screen: ƒ Screen()ScreenOrientation: ƒ ScreenOrientation()ScriptProcessorNode: ƒ ScriptProcessorNode()SecurityPolicyViolationEvent: ƒ SecurityPolicyViolationEvent()Selection: ƒ Selection()Sensor: ƒ Sensor()SensorErrorEvent: ƒ SensorErrorEvent()ServiceWorker: ƒ ServiceWorker()ServiceWorkerContainer: ƒ ServiceWorkerContainer()ServiceWorkerRegistration: ƒ ServiceWorkerRegistration()Set: ƒ Set()ShadowRoot: ƒ ShadowRoot()SharedArrayBuffer: ƒ SharedArrayBuffer()SharedWorker: ƒ SharedWorker()SourceBuffer: ƒ SourceBuffer()SourceBufferList: ƒ SourceBufferList()SpeechSynthesisErrorEvent: ƒ SpeechSynthesisErrorEvent()SpeechSynthesisEvent: ƒ SpeechSynthesisEvent()SpeechSynthesisUtterance: ƒ SpeechSynthesisUtterance()StaticRange: ƒ StaticRange()StereoPannerNode: ƒ StereoPannerNode()Storage: ƒ Storage()StorageEvent: ƒ StorageEvent()StorageManager: ƒ StorageManager()String: ƒ String()StylePropertyMap: ƒ StylePropertyMap()StylePropertyMapReadOnly: ƒ StylePropertyMapReadOnly()StyleSheet: ƒ StyleSheet()StyleSheetList: ƒ StyleSheetList()SubtleCrypto: ƒ SubtleCrypto()Symbol: ƒ Symbol()SyncManager: ƒ SyncManager()SyntaxError: ƒ SyntaxError()TaskAttributionTiming: ƒ TaskAttributionTiming()Text: ƒ Text()TextDecoder: ƒ TextDecoder()TextDecoderStream: ƒ TextDecoderStream()TextEncoder: ƒ TextEncoder()TextEncoderStream: ƒ TextEncoderStream()TextEvent: ƒ TextEvent()TextMetrics: ƒ TextMetrics()TextTrack: ƒ TextTrack()TextTrackCue: ƒ TextTrackCue()TextTrackCueList: ƒ TextTrackCueList()TextTrackList: ƒ TextTrackList()TimeRanges: ƒ TimeRanges()Touch: ƒ Touch()TouchEvent: ƒ TouchEvent()TouchList: ƒ TouchList()TrackEvent: ƒ TrackEvent()TransformStream: ƒ TransformStream()TransitionEvent: ƒ TransitionEvent()TreeWalker: ƒ TreeWalker()TypeError: ƒ TypeError()UIEvent: ƒ UIEvent()URIError: ƒ URIError()URL: ƒ URL()URLSearchParams: ƒ URLSearchParams()USB: ƒ USB()USBAlternateInterface: ƒ USBAlternateInterface()USBConfiguration: ƒ USBConfiguration()USBConnectionEvent: ƒ USBConnectionEvent()USBDevice: ƒ USBDevice()USBEndpoint: ƒ USBEndpoint()USBInTransferResult: ƒ USBInTransferResult()USBInterface: ƒ USBInterface()USBIsochronousInTransferPacket: ƒ USBIsochronousInTransferPacket()USBIsochronousInTransferResult: ƒ USBIsochronousInTransferResult()USBIsochronousOutTransferPacket: ƒ USBIsochronousOutTransferPacket()USBIsochronousOutTransferResult: ƒ USBIsochronousOutTransferResult()USBOutTransferResult: ƒ USBOutTransferResult()Uint8Array: ƒ Uint8Array()Uint8ClampedArray: ƒ Uint8ClampedArray()Uint16Array: ƒ Uint16Array()Uint32Array: ƒ Uint32Array()UserActivation: ƒ UserActivation()VTTCue: ƒ VTTCue()ValidityState: ƒ ValidityState()VisualViewport: ƒ VisualViewport()WaveShaperNode: ƒ WaveShaperNode()WeakMap: ƒ WeakMap()WeakSet: ƒ WeakSet()WebAssembly: WebAssembly {compile: ƒ, validate: ƒ, instantiate: ƒ, compileStreaming: ƒ, instantiateStreaming: ƒ, …}WebGL2RenderingContext: ƒ WebGL2RenderingContext()WebGLActiveInfo: ƒ WebGLActiveInfo()WebGLBuffer: ƒ WebGLBuffer()WebGLContextEvent: ƒ WebGLContextEvent()WebGLFramebuffer: ƒ WebGLFramebuffer()WebGLProgram: ƒ WebGLProgram()WebGLQuery: ƒ WebGLQuery()WebGLRenderbuffer: ƒ WebGLRenderbuffer()WebGLRenderingContext: ƒ WebGLRenderingContext()WebGLSampler: ƒ WebGLSampler()WebGLShader: ƒ WebGLShader()WebGLShaderPrecisionFormat: ƒ WebGLShaderPrecisionFormat()WebGLSync: ƒ WebGLSync()WebGLTexture: ƒ WebGLTexture()WebGLTransformFeedback: ƒ WebGLTransformFeedback()WebGLUniformLocation: ƒ WebGLUniformLocation()WebGLVertexArrayObject: ƒ WebGLVertexArrayObject()WebKitCSSMatrix: ƒ DOMMatrix()WebKitMutationObserver: ƒ MutationObserver()WebSocket: ƒ WebSocket()WebView: class extendsWheelEvent: ƒ WheelEvent()Window: ƒ Window()Worker: ƒ Worker()Worklet: ƒ Worklet()WritableStream: ƒ WritableStream()XMLDocument: ƒ XMLDocument()XMLHttpRequest: ƒ XMLHttpRequest()XMLHttpRequestEventTarget: ƒ XMLHttpRequestEventTarget()XMLHttpRequestUpload: ƒ XMLHttpRequestUpload()XMLSerializer: ƒ XMLSerializer()XPathEvaluator: ƒ XPathEvaluator()XPathExpression: ƒ XPathExpression()XPathResult: ƒ XPathResult()XSLTProcessor: ƒ XSLTProcessor()console: console {debug: ƒ, error: ƒ, info: ƒ, log: ƒ, warn: ƒ, …}decodeURI: ƒ decodeURI()decodeURIComponent: ƒ decodeURIComponent()encodeURI: ƒ encodeURI()encodeURIComponent: ƒ encodeURIComponent()escape: ƒ escape()eval: ƒ eval()event: undefinedglobalThis: Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}isFinite: ƒ isFinite()isNaN: ƒ isNaN()offscreenBuffering: trueparseFloat: ƒ parseFloat()parseInt: ƒ parseInt()undefined: undefinedunescape: ƒ unescape()webkitMediaStream: ƒ MediaStream()webkitRTCPeerConnection: ƒ RTCPeerConnection()webkitSpeechGrammar: ƒ SpeechGrammar()webkitSpeechGrammarList: ƒ SpeechGrammarList()webkitSpeechRecognition: ƒ SpeechRecognition()webkitSpeechRecognitionError: ƒ SpeechRecognitionError()webkitSpeechRecognitionEvent: ƒ SpeechRecognitionEvent()webkitURL: ƒ URL()__proto__: Window
// // VM1394:2 Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}
//
// [12, 4312, 54343, 5345].forEach(function (x) {
//     console.log(this)
// }, Math)
// // Math {abs: ƒ, acos: ƒ, acosh: ƒ, asin: ƒ, asinh: ƒ, …}E: 2.718281828459045LN2: 0.6931471805599453LN10: 2.302585092994046LOG2E: 1.4426950408889634LOG10E: 0.4342944819032518PI: 3.141592653589793SQRT1_2: 0.7071067811865476SQRT2: 1.4142135623730951abs: ƒ abs()acos: ƒ acos()acosh: ƒ acosh()asin: ƒ asin()asinh: ƒ asinh()atan: ƒ atan()atan2: ƒ atan2()atanh: ƒ atanh()cbrt: ƒ cbrt()ceil: ƒ ceil()clz32: ƒ clz32()cos: ƒ cos()cosh: ƒ cosh()exp: ƒ exp()expm1: ƒ expm1()floor: ƒ floor()fround: ƒ fround()hypot: ƒ hypot()imul: ƒ imul()log: ƒ log()log1p: ƒ log1p()log2: ƒ log2()log10: ƒ log10()max: ƒ max()min: ƒ min()pow: ƒ pow()random: ƒ random()round: ƒ round()sign: ƒ sign()sin: ƒ sin()sinh: ƒ sinh()sqrt: ƒ sqrt()tan: ƒ tan()tanh: ƒ tanh()trunc: ƒ trunc()Symbol(Symbol.toStringTag): "Math"__proto__: Object
// // VM1410:2 Math {abs: ƒ, acos: ƒ, acosh: ƒ, asin: ƒ, asinh: ƒ, …}
// // VM1410:2 Math {abs: ƒ, acos: ƒ, acosh: ƒ, asin: ƒ, asinh: ƒ, …}
// // VM1410:2 Math {abs: ƒ, acos: ƒ, acosh: ƒ, asin: ƒ, asinh: ƒ, …}
// [12, 4312, 54343, 5345].forEach(function (x) {
//     console.log(this)
// }, Number)
// ƒ Number() { [native code] }
[12, 4312, 54343, 5345].forEach(x => {
    console.log(this)
})
// {}
// {}
// {}
// {}
```

## array

- 创建一个全为空的数组
- 创建一个带有值的数组

```js
console.log(Array.apply(null, {length: 3}))// [ undefined, undefined, undefined ]

console.log(Array.apply(null, {'0': 1, length: 3}))// [ 1, undefined, undefined ]

console.log(Array.apply(null, {'2': 1, length: 3}))// [ undefined, undefined, 1 ]

```
## bitwiseOperators

>位操作随机生成字符串待研究
```js
//字符串转数字
console.log(+'1')//1
console.log("1" * 1)//1
console.log("1" | 0)//1
console.log("1" >> 0)//1
console.log("1" << 0)//1

// 生成一个10位随机字符串

let n = 10;
var str = 'qwertyuiopaaaasdfghjkmnbvcxz1235456789'
var res = ''
for (let i = 0; i < n; i++) {
    res += str[parseInt(Math.random() * (str.length + 1))]
}
console.log(res)//rkjhufinu3

```

## this
this的作用域:普通函数中,this指向的被调用的那个函数或对象;箭头函数中,this指向实例顶层
```js
const a = {
  name: 'aaa',
  greeting: function () {
    return this.name
  }
}

const b = {
  name: 'bbb',
  greeting: a.greeting
}

console.log(b.greeting())//bbb
console.log(a.greeting.call(b))//bbb
console.log(a.greeting.apply(b))//bbb
console.log(a.greeting.bind(b)())//bbb
const a = {
  name: 'aaa',
  greeting:  ()=> {
    return this.name
  }
}

const b = {
  name: 'bbb',
  greeting: a.greeting
}

console.log(b.greeting())//undefined
console.log(a.greeting.call(b))//undefined
console.log(a.greeting.apply(b))//undefined
console.log(a.greeting.bind(b)())//undefined

```
![avatar](./js/public/this.png)
# 你不知道的Vue

## mixins-extends研究
很多人都用过mixins和extends,但是偏偏就对mixins和extend的执行顺序不慎了解.
那么开启我们的研究吧.


mixins

![avatar](./vue/public/mixins.png)

extends

![avatar](./vue/public/extend.png)

>官网解释:mixins可以接受一个对象数组,而extend只能扩展一个组件/对象/函数

### mixins和extends执行顺序

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/vue/2.5.17-beta.0/vue.min.js"></script>
</head>
<body>
<div id="app">
    <p>mixin的data值为:{{mixinData}}</p>
    <button @click="handleClick()">点我触发handleClick方法</button>
</div>
</body>
<script>
    var mixin = {
        data: {mixinData: '我是mixin的data'},
        created() {
            console.log('这是mixin的created方法')
        },
        methods: {
            handleClick() {
                console.log('这是mixin的handleClick方法')
            }
        }
    }
    var extend = {
        data: {extendData: '我是extend的data'},
        created() {
            console.log('这是extend的created方法')
        },
        methods: {
            handleClick() {
                console.log('这是extend的handleClick方法')
            }
        }
    }

    var vm = new Vue({
        el: '#app',
        data: {mixinData: '我是vue实例里的data'},
        created() {
            console.log('这是vue实例里的created方法')
        },
        methods: {
            handleClick() {
                console.log('这是vue实例里的handleClick方法')
            }
        },
        mixins: [mixin],
        extends: extend
    })
</script>
</html>

```
结果

![avatar](./vue/public/mixins-extends.png)

```text
这是extend的created方法
这是mixin的created方法
这是vue实例里的created方法
这是vue实例里的handleClick方法
```

### 多个mixins和extends执行顺序(mixin在前,mixin2在后)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/vue/2.5.17-beta.0/vue.min.js"></script>
</head>
<body>
<div id="app">
    <p>mixin的data值为:{{mixinData}}</p>
    <button @click="handleClick()">点我触发handleClick方法</button>
</div>
</body>
<script>
    var extend = {
        data: {extendData: '我是extend的data'},
        created() {
            console.log('这是extend的created方法')
        },
        methods: {
            handleClick() {
                console.log('这是extend的handleClick方法')
            }
        }
    }

    var mixin = {
        data: {mixinData: '我是mixin的data'},
        created() {
            console.log('这是mixin的created方法')
        },
        methods: {
            handleClick() {
                console.log('这是mixin的handleClick方法')
            }
        }
    }
    var mixin2 = {
        data: {mixinData: '我是mixin2的data'},
        created() {
            console.log('这是mixin2的created方法')
        },
        methods: {
            handleClick() {
                console.log('这是mixin2的handleClick方法')
            }
        }
    }

    var vm = new Vue({
        el: '#app',
        data: {mixinData: '我是vue实例里的data'},
        created() {
            console.log('这是vue实例里的created方法')
        },
        methods: {
            handleClick() {
                console.log('这是vue实例里的handleClick方法')
            }
        },
        extends: extend,
        mixins: [mixin, mixin2]
    })
</script>
</html>

```
结果

![avatar](./vue/public/mixins-extends.png)

```text
这是extend的created方法
这是mixin的created方法
这是mixin2的created方法
这是vue实例里的created方法
这是vue实例里的handleClick方法
```

### 多个mixins和extends执行顺序(mixin在后,mixin2在前)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/vue/2.5.17-beta.0/vue.min.js"></script>
</head>
<body>
<div id="app">
    <p>mixin的data值为:{{mixinData}}</p>
    <button @click="handleClick()">点我触发handleClick方法</button>
</div>
</body>
<script>
    var extend = {
        data: {extendData: '我是extend的data'},
        created() {
            console.log('这是extend的created方法')
        },
        methods: {
            handleClick() {
                console.log('这是extend的handleClick方法')
            }
        }
    }

    var mixin = {
        data: {mixinData: '我是mixin的data'},
        created() {
            console.log('这是mixin的created方法')
        },
        methods: {
            handleClick() {
                console.log('这是mixin的handleClick方法')
            }
        }
    }
    var mixin2 = {
        data: {mixinData: '我是mixin2的data'},
        created() {
            console.log('这是mixin2的created方法')
        },
        methods: {
            handleClick() {
                console.log('这是mixin2的handleClick方法')
            }
        }
    }

    var vm = new Vue({
        el: '#app',
        data: {mixinData: '我是vue实例里的data'},
        created() {
            console.log('这是vue实例里的created方法')
        },
        methods: {
            handleClick() {
                console.log('这是vue实例里的handleClick方法')
            }
        },
        extends: extend,
        mixins: [mixin2, mixin]
    })
</script>
</html>

```
结果

![avatar](./vue/public/mixins-extends.png)

```text
这是extend的created方法
这是mixin2的created方法
这是mixin的created方法
这是vue实例里的created方法
这是vue实例里的handleClick方法
```

### 多个生命周期
```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/vue/2.5.17-beta.0/vue.min.js"></script>
</head>
<body>
<div id="app">
    <p>mixin的data值为:{{mixinData}}</p>
    <button @click="handleClick()">点我触发handleClick方法</button>
</div>
</body>
<script>
    var extend = {
        data: {extendData: '我是extend的data'},
        beforeCreate(){
            console.log('这是extend的beforeCreate方法')
        },
        created() {
            console.log('这是extend的created方法')
        },
        beforeMount(){
            console.log('这是extend的beforeMount方法')
        },
        mounted(){
            console.log('这是extend的mounted方法')
        },
        beforeUpdate(){
            console.log('这是extend的beforeUpdate方法')
        },
        updated(){
            console.log('这是extend的updated方法')
        },
        methods: {
            handleClick() {
                console.log('这是extend的handleClick方法')
            }
        }
    }

    var mixin = {
        data: {mixinData: '我是mixin的data'},
        beforeCreate(){
            console.log('这是mixin的beforeCreate方法')
        },
        created() {
            console.log('这是mixin的created方法')
        },
        beforeMount(){
            console.log('这是mixin的beforeMount方法')
        },
        mounted(){
            console.log('这是mixin的mounted方法')
        },
        beforeUpdate(){
            console.log('这是mixin的beforeUpdate方法')
        },
        updated(){
            console.log('这是mixin的updated方法')
        },
        methods: {
            handleClick() {
                console.log('这是mixin的handleClick方法')
            }
        }
    }
    var mixin2 = {
        data: {mixinData: '我是mixin2的data'},
        beforeCreate(){
            console.log('这是mixin2的beforeCreate方法')
        },
        created() {
            console.log('这是mixin2的created方法')
        },
        beforeMount(){
            console.log('这是mixin2的beforeMount方法')
        },
        mounted(){
            console.log('这是mixin2的mounted方法')
        },
        beforeUpdate(){
            console.log('这是mixin2的beforeUpdate方法')
        },
        updated(){
            console.log('这是mixin2的updated方法')
        },
        methods: {
            handleClick() {
                console.log('这是mixin2的handleClick方法')
            }
        }
    }

    var vm = new Vue({
        el: '#app',
        data: {mixinData: '我是vue实例里的data'},
        beforeCreate(){
            console.log('这是vue实例里的beforeCreate方法')
        },
        created() {
            console.log('这是vue实例里的created方法')
        },
        beforeMount(){
            console.log('这是vue实例里的beforeMount方法')
        },
        mounted(){
            console.log('这是vue实例里的mounted方法')
        },
        beforeUpdate(){
            console.log('这是vue实例里的beforeUpdate方法')
        },
        updated(){
            console.log('这是vue实例里的updated方法')
        },
        methods: {
            handleClick() {
                console.log('这是vue实例里的handleClick方法')
            }
        },
        extends: extend,
        mixins: [mixin2, mixin]
    })
</script>
</html>

```
结果

![avatar](./vue/public/mixins-extends.png)

```text
这是extend的beforeCreate方法
这是mixin2的beforeCreate方法
这是mixin的beforeCreate方法
这是vue实例里的beforeCreate方法
这是extend的created方法
这是mixin2的created方法
这是mixin的created方法
这是vue实例里的created方法
这是extend的beforeMount方法
这是mixin2的beforeMount方法
这是mixin的beforeMount方法
这是vue实例里的beforeMount方法
这是extend的mounted方法
这是mixin2的mounted方法
这是mixin的mounted方法
这是vue实例里的mounted方法
这是vue实例里的handleClick方法
```

### vue实例中没有重名方法和重名变量
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/vue/2.5.17-beta.0/vue.min.js"></script>
</head>
<body>
<div id="app">
    <p>mixin的data值为:{{mixinData}}</p>
    <button @click="handleClick()">点我触发handleClick方法</button>
</div>
</body>
<script>
    var extend = {
        data: {extendData: '我是extend的data'},
        beforeCreate(){
            console.log('这是extend的beforeCreate方法')
        },
        created() {
            console.log('这是extend的created方法')
        },
        beforeMount(){
            console.log('这是extend的beforeMount方法')
        },
        mounted(){
            console.log('这是extend的mounted方法')
        },
        beforeUpdate(){
            console.log('这是extend的beforeUpdate方法')
        },
        updated(){
            console.log('这是extend的updated方法')
        },
        methods: {
            handleClick() {
                console.log('这是extend的handleClick方法')
            }
        }
    }

    var mixin = {
        data: {mixinData: '我是mixin的data'},
        beforeCreate(){
            console.log('这是mixin的beforeCreate方法')
        },
        created() {
            console.log('这是mixin的created方法')
        },
        beforeMount(){
            console.log('这是mixin的beforeMount方法')
        },
        mounted(){
            console.log('这是mixin的mounted方法')
        },
        beforeUpdate(){
            console.log('这是mixin的beforeUpdate方法')
        },
        updated(){
            console.log('这是mixin的updated方法')
        },
        methods: {
            handleClick() {
                console.log('这是mixin的handleClick方法')
            }
        }
    }
    var mixin2 = {
        data: {mixinData: '我是mixin2的data'},
        beforeCreate(){
            console.log('这是mixin2的beforeCreate方法')
        },
        created() {
            console.log('这是mixin2的created方法')
        },
        beforeMount(){
            console.log('这是mixin2的beforeMount方法')
        },
        mounted(){
            console.log('这是mixin2的mounted方法')
        },
        beforeUpdate(){
            console.log('这是mixin2的beforeUpdate方法')
        },
        updated(){
            console.log('这是mixin2的updated方法')
        },
        methods: {
            handleClick() {
                console.log('这是mixin2的handleClick方法')
            }
        }
    }

    var vm = new Vue({
        el: '#app',
        beforeCreate(){
            console.log('这是vue实例里的beforeCreate方法')
        },
        created() {
            console.log('这是vue实例里的created方法')
        },
        beforeMount(){
            console.log('这是vue实例里的beforeMount方法')
        },
        mounted(){
            console.log('这是vue实例里的mounted方法')
        },
        beforeUpdate(){
            console.log('这是vue实例里的beforeUpdate方法')
        },
        updated(){
            console.log('这是vue实例里的updated方法')
        },
        extends: extend,
        mixins: [mixin2, mixin]
    })
</script>
</html>

```

结果:

![avatar](./vue/public/samename%20function-data.png)

```text
这是extend的beforeCreate方法
这是mixin2的beforeCreate方法
这是mixin的beforeCreate方法
这是vue实例里的beforeCreate方法
这是extend的created方法
这是mixin2的created方法
这是mixin的created方法
这是vue实例里的created方法
这是extend的beforeMount方法
这是mixin2的beforeMount方法
这是mixin的beforeMount方法
这是vue实例里的beforeMount方法
这是extend的mounted方法
这是mixin2的mounted方法
这是mixin的mounted方法
这是vue实例里的mounted方法
这是vue实例里的handleClick方法
这是mixin的handleClick方法
```

## 总结:

1. 先执行extends,再执行mixins,然后才是vue实例的生命周期;
2. 放入多个mixins,按mixins引用的顺序执行;
3. 不能放入两个乃至多个extends;
4. mixins/extends/vue实例生命周期执行顺序都是一样的,都是先一起执行完beforeCreate,然后在到created等生命周期;
5. vue实例里的方法会覆盖mixins和extends的重名方法和重名变量,如果vue实例中没有重名方法和重名变量,那么最后被引用的mixins会覆盖之前的mixins和extends方法和变量.
