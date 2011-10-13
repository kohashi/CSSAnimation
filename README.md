CSSAnimation
============

CSSアニメーションをjavascriptから。

- CSSAnimation.js - base API。普通の JavaScript です(ライブラリ等不要な、単体のJavaScriptです)。
- CSSAnimation.MooTools.js - MooTools API に機能を組み込みました。 使用には CSSAnimation.jsが必要。
- CSSAnimation.jQuery.js - jQuery に機能を組み込みました。

<!--
CSS Transforms and Transitions with JavaScript.

- CSSAnimation.js - Base, vanilla JavaScript implementation, no library required.
- CSSAnimation.MooTools.js - MooTools API to the base constructors, requires CSSAnimation.js.
- CSSAnimation.jQuery.js - jQuery API to the base constructors, requires CSSAnimation.js.
-->

動作サンプル 
--------------
	// jQuery 使用
	$('#some-el').setTransition({
	  property: 'transform',
	  'timing-function': 'ease-in',
	  duration: '2s'
	}).rotate(720).scale(2);
	
	// MooTools 使用
	$('some-el').setTransition({
	  property: 'transform',
	  'timing-function': 'ease-in',
	  duration: '2s'
	}).rotate(720).scale(2);
	
	// 素のJavaScript(ライブラリなし）
	var element = document.getElementById('some-el'),
	    transition = new Transition(element),
	    transform = new Transform(element),
	
	transition.set({
	  property: 'transform',
	  'timing-function': 'ease-in',
	  duration: '2s'
	});
	
	transform.rotate(720).scale(2);
	
	

<!--Quick Examples 
--------------
	// Vanilla JavaScript
	var element = document.getElementById('some-el'),
	    transition = new Transition(element),
	    transform = new Transform(element),
	
	transition.set({
	  property: 'transform',
	  'timing-function': 'ease-in',
	  duration: '2s'
	});
	
	transform.rotate(720).scale(2);
	
	// MooTools
	$('some-el').setTransition({
	  property: 'transform',
	  'timing-function': 'ease-in',
	  duration: '2s'
	}).rotate(720).scale(2);
	
	// jQuery
	$('#some-el').setTransition({
	  property: 'transform',
	  'timing-function': 'ease-in',
	  duration: '2s'
	}).rotate(720).scale(2);
-->

サポートブラウザ
---------------

- Safari
- Chrome
- Mozilla
- Opera
- IE 9 (仕様上は動作しますが、テストはしていません)

ただし、 transform と transition のサポートがあるバージョンに限ります.

<!--
- Safari
- Chrome
- Mozilla
- Opera
- IE 9 (theoretically, haven't tested yet)

Insofar as the version supports transform and transition.
-->


このリポジトリについて
---------------------

- 使用しているブランチは `develop`です。
- マスターブランチはタグ付されたリリースだけです。
- MooTools' Packager できました。（訳注：MooTools用のWebビルドツール。複数の.jsファイルをひとつにする）
- 寄付歓迎！（訳注：開発元は https://github.com/rpflorence/CSSAnimation です）


<!--About this Repository
---------------------

- Tracking branch is `develop`
- Master branch contains only tagged releases
- MooTools' Packager ready
- Contributors welcome!
- 
-->

License
-------

MIT Style license.

---

API リファレンス
=============

以下のドキュメントでは、 base API, MooTools, jQuery の順で表示します。  すべてのAPIは同じ引数を取ります。  Assume the following objects for the rest of the documentation:

	var element = document.getElementById('some-el'),
	    transition = new Transition(element),
	    transform = new Transform(element),
	
	    mooElement = document.id('some-el'),
	    jQueryCollection = jQuery('#some-el');

Transition
==========

CSS Transition を定義.

Transition method: constructor
------------------------------

 `Transition` オブジェクトを作成します。MooTools と jQuery では自動で作ります。

**Signature**

	new Transition(element);
	// MooTools と jQuery では自動で作ります。

**Arguments**

- element - DOMエレメント

**Example**

	var element = document.getElementById('some-el'),
	    transition = new Transition(element);

Transitions method: set
-----------------------

**Signature**
	// ルールと対応する値を設定します。
	transition.set(rule, value);
	transition.set({
	  rule: value,
	  rule2: value2
	});
	
	mooElement.setTransition
	jQueryCollection.setTransition

**Arguments**

- rule (string) - 以下の設定ができます:
	- `property` - CSSプロパティ名。  `transform`を指定する場合、 ブラウザプリフィックスが追加されます(i.e. -webkit-)。 カンマ区切りで複数指定もできます。
	- `duration` - i.e. `500ms`, `1s`.
	- `timing-function` - i.e. `ease`, `ease-out`, `ease-in-out`, `cubic-bezier(x1, y1, x2, y2)`

**Examples**

通常、すべてのルールを一度にセットします。:

	transition.set({
	  property: 'transform',
	  'timing-function': 'ease-out',
	  duration: '500ms'
	});

このように、任意に追加もできます。:

	transition.set('property', 'opacity');
	transition.set('duration', '0.5s');

	// 複数プロパティはカンマ区切りです
	transition.set('property', 'background-color, color');

	// allの指定で、すべてのプロパティを設定できます。
	transition.set('property', 'all');

要素のスタイルを変更すると、Transitionが消えます（訳注：未検証）。

	element.className = 'some-class-with-new-styles';
	element.style.marginTop = '50px';

あるいは、`Transform` と組み合わせて使います:

	transform.translate('x', 100);

Transition method: clear
------------------------

指定の、またはすべての transition ルールを消去します。

**Signature**

	transition.clear([rule]);

	mooElement.clearTransition;
	jQueryCollection.clearTransition;

**Arguments**

- rule (string) - 同じ値のルールも指定可能。nullの場合、すべてのルールを消去。

**Examples**

	transition.clear('timing-function'); //timing-functionをクリア
	transition.clear(); // すべてのルールをクリア
	
	mooElement.clearTransition('property');
	jQueryCollection.clearTransition('duration');

Transform
=========

DOMエレメントの `transform` プロパティを定義または管理する。

対象:

- `translateX`, `translateY`, `translateZ`
- `rotateX`, `rotateY`, `rotateZ`, `rotate`
- `scale`, `scaleX`, `scaleY`
- `skewX`, `skewY` (does not yet support just `skew(x, y)`)
- `matrix`

Transform method: constructor
------------------------------

新しい `Transform` オブジェクトを生成。 MooTools と jQuery では自動で作ります。

**Signature**

	new Transform(element);
	// MooTools と jQuery では自動で作ります。

**Arguments**

- element - DOMエレメント

**Example**

	var element = document.getElementById('some-el'),
	    transform = new Transform(element);

Transform method: translate
---------------------------

**Signature**

	transform.translate(axis, value);
	transform.translate({
		axis: value,
		axis2: value2
	});
	
	mooElement.translate
	jQueryCollection.translate

**Arguments**

- axis (string) - 軸方向。  つまり`x`, `y` を指定する。 Safari のみ `z` が指定可能。
- value (number) - 移動量をパーセント指定。 ただし `z` のみピクセル指定。 `Transform` は動的に単位を定義します。

**Examples**

	transform.translate('x', 100); // 右方向へ 100% 移動する
	transform.translate({  //右下へ50%移動する
		x: 50,
		y: 50
	});
	
	mooElement.translate('x', 100);
	jQueryCollection.translate('x', 100);

Transform method: rotate
------------------------

エレメントを指定した軸(x,y,z)で回転させる。

**Signature**

	transform.rotate(value);
	transform.rotate(axis, value);
	transform.rotate({
		axis: value,
		axis2: value2
	});
	
	mooElement.rotate;
	jQueryCollection.rotate;

**Arguments**

- axis (string) - 回転軸。 `z` の指定のみ許可（初期値）。 Safari のみ `x`, `y`  が指定可能。
- value (number) - 回転させる度数。30を指定すると30度回転。

**Examples**

	transform.rotate(-45); // z軸方向に、反時計回り45度回転
	transform.rotate('z', -45); // 上と同じ
	// Safari のみ 3D回転ができる
	transform.rotate({
		x: 30,
		y: 30
	});
	
	mooElement.rotate(-45);
	jQueryCollection.rotate(-45);

Transform method: scale
-----------------------

エレメントを拡大縮小する。

**Signature**

	transform.scale(value);
	transform.scale(axis, value);
	transform.scale({
		axis: value,
		axis2: value2
	});
	
	mooElement.scale;
	jQueryCollection.scale;

**Arguments**

- axis (string) - 拡大方向。  `x` , `y` を指定。省略時は両方向。
- value (number) - 拡大率。

**Examples**

	transform.scale(2); // タテヨコ2倍にする。
	transform.scale('x', 1.5); // 水平方向に1.5倍に変形。
	transform.scale({
		x: 0.5,
		y: 1.5
	});
	
	mooElement.scale(2);
	jQueryCollection.scale(2);

Transform method: skew
----------------------

エレメントを角度指定してゆがませます。

**Signature**

	transform.skew(axis, value);
	transform.skew({
		axis: value,
		axis2: value2
	});
	
	mooElement.skew;
	jQueryCollection.skew;

**Arguments**

- axis (string) - ゆがみ軸。 `x`, `y` の指定。
- value (number) - ゆがみ量を度数指定。

**Examples**

	transform.skew('x', 30);
	transform.scale({
		x: 30,
		y: -10
	});
	
	mooElement.skew('x', 30);
	jQueryCollection.skew('x', 30);

**Notes**

CSS transforms は `skew(x, y)` と `skew(x)` の両方を許可しています。しかし、現在このスクリプトはサポートしていません。  （今後対応予定です）

Transform method: matrix
------------------------

matrix指定の2D 変形を指定する。.

**Signature**

	transform.matrix(a, b, c, d, x, y);
	mooElement.matrix;
	jQueryCollection.matrix;

**Arguments**

Arguments combine to build the transform matrix. All arguments are numbers.

	|a c| |x|
	|b d| |y|

**Examples**

	transform.matrix(1, 0.1, 0.5, 1,  50, 50);
	mooElement.matrix(1, 0.1, 0.5, 1,  50, 50);
	jQueryCollection.matrix(1, 0.1, 0.5, 1,  50, 50);
<!--
API Reference
=============

The following documentation first shows the base API, then MooTools, then jQuery.  All APIs take the same arguments.  Assume the following objects for the rest of the documentation:

	var element = document.getElementById('some-el'),
	    transition = new Transition(element),
	    transform = new Transform(element),
	
	    mooElement = document.id('some-el'),
	    jQueryCollection = jQuery('#some-el');

Transition
==========

Define a CSS Transition for an element.

Transition method: constructor
------------------------------

Creates a new `Transition` object.  Constructed and cached automatically for MooTools and jQuery

**Signature**

	new Transition(element);
	// constructed and cached automatically for MooTools and jQuery

**Arguments**

- element - A DOM element reference

**Example**

	var element = document.getElementById('some-el'),
	    transition = new Transition(element);

Transitions method: set
-----------------------

**Signature**

	transition.set(rule, value);
	transition.set({
	  rule: value,
	  rule2: value2
	});
	
	mooElement.setTransition
	jQueryCollection.setTransition

**Arguments**

- rule (string) - can be any of the following:
	- `property` - the CSS property(s) to transition.  If `transform` is specified, the proper browser prefix will be added (i.e. -webkit-). Separate multiple by commas.
	- `duration` - i.e. `500ms`, `1s`.
	- `timing-function` - i.e. `ease`, `ease-out`, `ease-in-out`, `cubic-bezier(x1, y1, x2, y2)`

**Examples**

Typically you set all rules at once:

	transition.set({
	  property: 'transform',
	  'timing-function': 'ease-out',
	  duration: '500ms'
	});

But you can do them one-by-one as well:

	transition.set('property', 'opacity');
	transition.set('duration', '0.5s');

	// supports multiple properties
	transition.set('property', 'background-color, color');

	// and all properties
	transition.set('property', 'all');

Now, change the styles of the element and they will transition.

	element.className = 'some-class-with-new-styles';
	element.style.marginTop = '50px';

Or use it in conjunction with `Transform`:

	transform.translate('x', 100);

Transition method: clear
------------------------

Clears all, or one transition rule.

**Signature**

	transition.clear([rule]);

	mooElement.clearTransition;
	jQueryCollection.clearTransition;

**Arguments**

- rule (string) - Accepts the same values as `rule` in set.  If null, clears all rules.

**Examples**

	transition.clear('timing-function');
	transition.clear(); // clears all rules
	
	mooElement.clearTransition('property');
	jQueryCollection.clearTransition('duration');

Transform
=========

Defines and manages the state of the `transform` property of an element.

Supports:

- `translateX`, `translateY`, `translateZ`
- `rotateX`, `rotateY`, `rotateZ`, `rotate`
- `scale`, `scaleX`, `scaleY`
- `skewX`, `skewY` (does not yet support just `skew(x, y)`)
- `matrix`

Transform method: constructor
------------------------------

Creates a new `Transform` object. Constructed and cached automatically for MooTools and jQuery 

**Signature**

	new Transform(element);
	// constructed and cached automatically for MooTools and jQuery

**Arguments**

- element - A DOM element reference

**Example**

	var element = document.getElementById('some-el'),
	    transform = new Transform(element);

Transform method: translate
---------------------------

**Signature**

	transform.translate(axis, value);
	transform.translate({
		axis: value,
		axis2: value2
	});
	
	mooElement.translate
	jQueryCollection.translate

**Arguments**

- axis (string) - The axis to move along.  Accepts `x`, `y`, and Safari only `z`.
- value (number) - The amount to translate in `%`, except for `z` which is defined in pixels.  `Transform` defines the units dynamically.

**Examples**

	transform.translate('x', 100); // moves right 100%
	transform.translate({
		x: 50,
		y: 50
	});
	
	mooElement.translate('x', 100);
	jQueryCollection.translate('x', 100);

Transform method: rotate
------------------------

Rotate an element around an axis.

**Signature**

	transform.rotate(value);
	transform.rotate(axis, value);
	transform.rotate({
		axis: value,
		axis2: value2
	});
	
	mooElement.rotate;
	jQueryCollection.rotate;

**Arguments**

- axis (string) - The axis to rotate around.  Accepts 'z', and safari only `x` and `y`.
- value (number) - The degrees to rotate.

**Examples**

	transform.rotate(-45); // rotates element counter-clockwise around the z axis
	transform.rotate('z', -45); // identical to above
	// Safari only, rotates 3D
	transform.rotate({
		x: 30,
		y: 30
	});
	
	mooElement.rotate(-45);
	jQueryCollection.rotate(-45);

Transform method: scale
-----------------------

Scale an element.

**Signature**

	transform.scale(value);
	transform.scale(axis, value);
	transform.scale({
		axis: value,
		axis2: value2
	});
	
	mooElement.scale;
	jQueryCollection.scale;

**Arguments**

- axis (string) - The axis to scale.  Accepts `x` and `y`.
- value (number) - The multiplier to scale by.

**Examples**

	transform.scale(2); // doubles in size
	transform.scale('x', 1.5); // scretches horizontally
	transform.scale({
		x: 0.5,
		y: 1.5
	});
	
	mooElement.scale(2);
	jQueryCollection.scale(2);

Transform method: skew
----------------------

Skew an element's angles.

**Signature**

	transform.skew(axis, value);
	transform.skew({
		axis: value,
		axis2: value2
	});
	
	mooElement.skew;
	jQueryCollection.skew;

**Arguments**

- axis (string) - The axis to skew.  Accepts `x` and `y`.
- value (number) - The amount in degrees to skew.

**Examples**

	transform.skew('x', 30);
	transform.scale({
		x: 30,
		y: -10
	});
	
	mooElement.skew('x', 30);
	jQueryCollection.skew('x', 30);

**Notes**

CSS transforms accept `skew(x, y)` and `skew(x)`, but this script does not.  It will support this syntax eventually.

Transform method: matrix
------------------------

Specifies a 2D transformation matrix.

**Signature**

	transform.matrix(a, b, c, d, x, y);
	mooElement.matrix;
	jQueryCollection.matrix;

**Arguments**

Arguments combine to build the transform matrix. All arguments are numbers.

	|a c| |x|
	|b d| |y|

**Examples**

	transform.matrix(1, 0.1, 0.5, 1,  50, 50);
	mooElement.matrix(1, 0.1, 0.5, 1,  50, 50);
	jQueryCollection.matrix(1, 0.1, 0.5, 1,  50, 50);

-->


