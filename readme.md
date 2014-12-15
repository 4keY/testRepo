<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<title>Hotkeys (Mouse, Joystick and Keyboard Shortcuts)</title>
<meta name="description" content="Free utility to create hotkeys, shortcuts, and macros for keyboard, mouse, and joystick. Any combination of keys and buttons can become a hotkey.">
<meta name="keywords" content="hotkey,hotkeys,hot key,hot keys,shortcut,shortcuts,shortcut key,shortcut keys,keyboard shortcut,keyboard shortcuts,button,buttons,click,press">
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<link href="static/theme.css" rel="stylesheet" type="text/css" />
<script src="static/jquery.js" type="text/javascript"></script>
<script src="static/tree.jquery.js" type="text/javascript"></script>
<script src="static/content.js" type="text/javascript"></script>
</head>
<body>

# Hotkeys (Mouse, Joystick and Keyboard Shortcuts)

## Table of Contents

*   [Introduction and Simple Examples](#Intro)
*   [Table of  Hotkey Prefix Symbols (Modifiers)](#Symbols)
*   [Context-sensitive Hotkeys](#Context)
*   [Custom Combinations and Other Features](#Features)
*   [Mouse Wheel Hotkeys](#Wheel)
*   [Hotkey Tips and Remarks](#Remarks)

## Introduction and Simple Examples

Hotkeys are sometimes referred to as shortcut keys because of their ability to easily trigger an action (such as launching a program or [keyboard macro](misc/Macros.htm)). In the following example, the hotkey Win+N is configured to launch Notepad. The pound sign [#] stands for the Windows key, which is known as a _modifier_:

<pre>#n::
Run Notepad
return</pre>

In the final line above,  `[return](commands/Return.htm)` serves to finish the hotkey. However, if a hotkey needs to execute only a single line, that line can be listed to the right of the double-colon. In other words, the `return` is implicit:

<pre>#n::Run Notepad</pre>

To use more than one modifier with a hotkey, list them consecutively (the order does not matter). The following example uses `^!s` to indicate Control+Alt+S:

<pre>^!s::
[Send](commands/Send.htm) Sincerely,{enter}John Smith  _; This line sends keystrokes to the active (foremost) window._
return</pre>

### You can use the following modifier symbols to define hotkeys:

<table class="info">
  <tr>
    <th>Symbol</th>
    <th>Description</th>
  </tr>
  <tr>
    <td width="30">**#**</td>
    <td>Win (Windows logo key). In v1.0.48.01+, for Windows Vista and later, hotkeys that include the Windows key (e.g. #a) will wait for the Windows key to be released before sending any text containing an &quot;L&quot; keystroke. This prevents usages of [Send](commands/Send.htm) within such a hotkey from locking the PC. This behavior applies to all sending modes except [SendPlay](commands/Send.htm#SendPlayDetail) (which doesn't need it) and [blind mode](commands/Send.htm#blind).</td>
  </tr>
  <tr>
    <td>**!**</td>
    <td>Alt</td>
  </tr>
  <tr>
    <td>**^**</td>
    <td>Control</td>
  </tr>
  <tr>
    <td>**+**</td>
    <td>Shift</td>
  </tr>
  <tr>
    <td>**&amp;**</td>
    <td>An ampersand may be used between any two keys or mouse buttons to combine them into a custom hotkey. See [below](#combo) for details.</td>
  </tr>
  <tr>
    <td>**&lt;**</td>
    <td><a name="LeftRight"></a>Use the left key of the pair. e.g. &lt;!a is the same as !a except that only the left Alt key will trigger it.</td>
  </tr>
  <tr>
    <td>**&gt;**</td>
    <td>Use the right key of the pair.</td>
  </tr>
  <tr>
    <td>**&lt;^&gt;!**</td>
    <td>

<a name="AltGr"></a>AltGr (alternate graving). If your keyboard layout has an AltGr key instead of a right-Alt key, this series of symbols can usually be used to stand for AltGr. For example:

      <pre>&lt;^&gt;!m::MsgBox You pressed AltGr+m.
&lt;^&lt;!m::MsgBox You pressed LeftControl+LeftAlt+m.</pre>

Alternatively, to make AltGr itself into a hotkey, use the following hotkey (without any hotkeys like the above present):

      <pre>LControl &amp; RAlt::MsgBox You pressed AltGr itself.</pre></td>
  </tr>
  <tr>
    <td>*****</td>
    <td>

<a name="wildcard"></a>Wildcard: Fire the hotkey even if extra modifiers are being held down. This is often used in conjunction with [remapping](misc/Remap.htm) keys or buttons. For example:

      <pre>*#c::Run Calc.exe  _; Win+C, Shift+Win+C, Ctrl+Win+C, etc. will all trigger this hotkey._
*ScrollLock::Run Notepad  _; Pressing ScrollLock will trigger this hotkey even when modifier key(s) are down._</pre></td>
  </tr>
  <tr>
    <td>**~**</td>
    <td>

<a name="Tilde"></a>When the hotkey fires, its key's native function will not be blocked (hidden from the system). In both of the below examples, the user's click of the mouse button will be sent to the active window:

      <pre>~RButton::MsgBox You clicked the right mouse button.
~RButton &amp; C::MsgBox You pressed C while holding down the right mouse button.</pre>

Notes: 1) Unlike the other prefix symbols, the tilde prefix is allowed to be present on some of a hotkey's [variants](commands/_IfWinActive.htm#variant) but absent on others; 2) Special hotkeys that are substitutes for [alt-tab](#alttab) always ignore the tilde prefix.

<span class="ver">[v1.1.14+]:</span> If the tilde prefix is attached to a custom modifier key which is also used as its own hotkey, that hotkey will fire when the key is pressed instead of being delayed until the key is released.  For example, the _~RButton_ hotkey above is fired as soon as the button is pressed.  Prior to v1.1.14 (or without the tilde prefix), it was fired when the button was released, but only if the _RButton &amp; C_ combination was not activated.

    </td>
  </tr>
  <tr id="prefixdollar">
    <td>**$**</td>
    <td>

This is usually only necessary if the script uses the [Send](commands/Send.htm) command to send the keys that comprise the hotkey itself, which might otherwise cause it to trigger itself. The $ prefix forces the [keyboard hook](commands/_InstallKeybdHook.htm) to be used to implement this hotkey, which as a side-effect prevents the [Send](commands/Send.htm) command from triggering it. The $ prefix is equivalent to having specified `[#UseHook](commands/_UseHook.htm)` somewhere above the definition of this hotkey.

<span class="ver">[v1.1.06+]:</span> [#InputLevel](commands/_InputLevel.htm) and [SendLevel](commands/SendLevel.htm) provide additional control over which hotkeys and hotstrings are triggered by the Send command.

    </td>
  </tr>
  <tr>
    <td>UP</td>
    <td>

<a name="keyup"></a>The word UP may follow the name of a hotkey to cause the hotkey to fire upon release of the key rather than when the key is pressed down. The following example [remaps](misc/Remap.htm) LWin to become LControl:

      <pre>*LWin::Send {LControl Down}
*LWin **Up**::Send {LControl Up}
</pre>

&quot;Up&quot; can also be used with normal hotkeys as in this example: `^!r Up::MsgBox You pressed and released Ctrl+Alt+R`. It also works with [combination hotkeys](#combo) (e.g. `F1 & e Up::`)

Limitations: 1) &quot;Up&quot; does not work with [joystick buttons](KeyList.htm); and 2) An &quot;Up&quot; hotkey without a normal/down counterpart hotkey will completely take over that key to prevent it from getting stuck down. One way to prevent this is to add a [tilde prefix](#Tilde) (e.g. `**~**LControl up::`)

On a related note, a technique similar to the above is to make a hotkey into a prefix key. The advantage is that although the hotkey will fire upon release, it will do so only if you did not press any other key while it was held down. For example:

      <pre>LControl &amp; F1::return  _; Make left-control a prefix by using it in front of &quot;&amp;&quot; at least once._
LControl::MsgBox You released LControl without having used it to modify any other key.</pre></td>
  </tr>
</table>

**(See the [Key List](KeyList.htm) for a complete list of keyboard keys and mouse/joystick buttons)**

Multiple hotkeys can be stacked vertically to have them perform the same action. For example:

<pre>^Numpad0::
^Numpad1::
MsgBox Pressing either Control+Numpad0 or Control+Numpad1 will display this message.
return</pre>

A key or key-combination can be disabled for the entire system by having it do nothing. The following example disables the right-side Windows key:

<pre>RWin::return</pre>

## Context-sensitive Hotkeys

The directives [#IfWinActive/Exist](commands/_IfWinActive.htm) and [#If](commands/_If.htm) can be used to make a hotkey perform a different action (or none at all) depending on a specific condition. For example:

<pre>#IfWinActive, ahk_class Notepad
^a::MsgBox You pressed Ctrl-A while Notepad is active. Pressing Ctrl-A in any other window will pass the Ctrl-A keystroke to that window.
#c::MsgBox You pressed Win-C while Notepad is active.

#IfWinActive
#c::MsgBox You pressed Win-C while any window except Notepad is active.

#If [MouseIsOver](commands/_If.htm#Examples)("ahk_class Shell_TrayWnd")
WheelUp::Send {Volume_Up}     _; Wheel over taskbar: increase/decrease volume._
WheelDown::Send {Volume_Down} _;_
</pre>

## Custom Combinations and Other Features

<a name="combo"></a>You can define a custom combination of two keys (except joystick buttons) by using &quot; &amp; &quot; between them. In the below example, you would hold down Numpad0 then press the second key to trigger the hotkey:

<pre>Numpad0 **&amp;** Numpad1::MsgBox You pressed Numpad1 while holding down Numpad0.
Numpad0 **&amp;** Numpad2::Run Notepad</pre>

<a name="prefix"></a>In the above example, Numpad0 becomes a _prefix key_; but this also causes Numpad0 to lose its original/native function when it is pressed by itself. To avoid this, a script may configure Numpad0 to perform a new action such as one of the following:

<pre>Numpad0::WinMaximize A   _; Maximize the active/foreground window._
Numpad0::Send {Numpad0}  _; Make the _release_ of Numpad0 produce a Numpad0 keystroke. See comment below._</pre>

The presence of one of the above custom combination hotkeys causes the _release_ of Numpad0 to perform the indicated action, but only if you did not press any other keys while Numpad0 was being held down.  In v1.1.14+, this behaviour can be avoided by applying the [tilde prefix](#Tilde) to either hotkey.

**Numlock, Capslock, and Scrolllock:** These keys may be forced to be &quot;AlwaysOn&quot; or &quot;AlwaysOff&quot;. For example: `[SetNumlockState](commands/SetNumScrollCapsLockState.htm) AlwaysOn`.

**Overriding Explorer's hotkeys:** Windows' built-in hotkeys such as Win-E (#e) and Win-R (#r) can be individually overridden simply by assigning them to an action in the script. See the [override page](misc/Override.htm) for details.

<a name="alttab" id="alttab"></a>**Substitutes for Alt-Tab:** Hotkeys can provide an alternate means of alt-tabbing. For example, the following two hotkeys allow you to alt-tab with your right hand:

<pre>RControl &amp; RShift::AltTab  _; Hold down right-control then press right-shift repeatedly to move forward._
RControl &amp; Enter::ShiftAltTab  _; Without even having to release right-control, press Enter to reverse direction._</pre>

For more details, see [Alt-Tab](#AltTabDetail).

## Mouse Wheel Hotkeys

Hotkeys that fire upon turning the mouse wheel are supported via the key names WheelDown and WheelUp. WheelLeft and WheelRight are also supported in v1.0.48+, but have no effect on operating systems older than Windows Vista. Here are some examples of mouse wheel hotkeys:

<pre>MButton &amp; WheelDown::MsgBox You turned the mouse wheel down while holding down the middle button.
^!WheelUp::MsgBox You rotated the wheel up while holding down Control+Alt.</pre>

In v1.0.43.03+, the built-in variable **A_EventInfo** contains the amount by which the wheel was turned, which is typically 1. However, A_EventInfo can be greater or less than 1 under the following circumstances:

*   If the mouse hardware reports distances of less than one notch, A_EventInfo may contain 0;
*   If the wheel is being turned quickly (depending on type of mouse), A_EventInfo may be greater than 1. A hotkey like the following can help analyze your mouse: `~WheelDown::ToolTip %A_EventInfo%`.

Some of the most useful hotkeys for the mouse wheel involve alternate modes of scrolling a window's text. For example, the following pair of hotkeys scrolls horizontally instead of vertically when you turn the wheel while holding down the left Control key:

<pre>~LControl &amp; WheelUp::  _; Scroll left._
ControlGetFocus, fcontrol, A
Loop 2  _; &lt;-- Increase this value to scroll faster._
    SendMessage, 0x114, 0, 0, %fcontrol%, A  _; 0x114 is WM_HSCROLL and the 0 after it is SB_LINELEFT._
return

~LControl &amp; WheelDown::  _; Scroll right._
ControlGetFocus, fcontrol, A
Loop 2  _; &lt;-- Increase this value to scroll faster._
    SendMessage, 0x114, 1, 0, %fcontrol%, A  _; 0x114 is WM_HSCROLL and the 1 after it is SB_LINERIGHT._
return</pre>

Finally, since mouse wheel hotkeys generate only down-events (never up-events), they cannot be used as [key-up hotkeys](#keyup).

## Hotkey Tips and Remarks

Each numpad key can be made to launch two different hotkey subroutines depending on the state of Numlock. Alternatively, a numpad key can be made to launch the same subroutine regardless of the Numlock state. For example:

<pre>NumpadEnd::
Numpad1::
MsgBox, This hotkey is launched regardless of whether Numlock is on.
return</pre>

If the tilde (~) operator is used with a [prefix key](#prefix) even once, that prefix will always be sent through to the active window. For example, in both of the below hotkeys, the active window will receive all right-clicks even though only one of the definitions contains a tilde:

<pre>~RButton &amp; LButton::MsgBox You pressed the left mouse button while holding down the right.
RButton &amp; WheelUp::MsgBox You turned the mouse wheel up while holding down the right button.</pre>

The [Suspend](commands/Suspend.htm) command can temporarily disable all hotkeys except for ones you make exempt. For greater selectivity, use [#IfWinActive/Exist](commands/_IfWinActive.htm).

By means of the [Hotkey](commands/Hotkey.htm) command, hotkeys can be created dynamically while the script is running. The Hotkey command can also modify, disable, or enable the script's existing hotkeys individually.

Joystick hotkeys do not currently support modifier prefixes such as ^ (Control) and # (Win). However, you can use [GetKeyState](commands/GetKeyState.htm) to mimic this effect as shown in the following example:

<pre>Joy2::
if not GetKeyState(&quot;Control&quot;)  _; Neither the left nor right Control key is down._
    return  _; i.e. Do nothing._
MsgBox You pressed the first joystick's second button while holding down the Control key.
return</pre>

There may be times when a hotkey should wait for its own modifier keys to be released before continuing. Consider the following example:

<pre>^!s::Send {Delete}</pre>

Pressing Control-Alt-S would cause the system to behave as though you pressed Control-Alt-Delete (due to the system's aggressive detection of Ctrl-Alt-Delete). To work around this, use [KeyWait](commands/KeyWait.htm) to wait for the keys to be released; for example:

<pre>^!s::
KeyWait Control
KeyWait Alt
Send {Delete}
return</pre>

If a hotkey label like `#z::` produces an error like &quot;Invalid Hotkey&quot;, your system's keyboard layout/language might not have the specified character (&quot;Z&quot; in this case). Try using a different character that you know exists in your keyboard layout.

A hotkey label can be used as the target of a [Gosub](commands/Gosub.htm) or [Goto](commands/Goto.htm). For example: `Gosub ^!s`.

One common use for hotkeys is to start and stop a repeating action, such as a series of keystrokes or mouse clicks. For an example of this, see [this FAQ topic](FAQ.htm#repeat).

Finally, each script is [quasi multi-threaded](misc/Threads.htm), which allows a new hotkey to be launched even when a previous hotkey subroutine is still running. For example, new hotkeys can be launched even while a [MsgBox](commands/MsgBox.htm) is being displayed by the current hotkey.

## Alt-Tab Hotkeys

Each Alt-Tab hotkey must be a combination of two keys, which is typically achieved via the ampersand symbol (&amp;). In the following example, you would hold down the right Alt key and press J or K to navigate the alt-tab menu:

<pre>RAlt &amp; j::AltTab
RAlt &amp; k::ShiftAltTab</pre>

_AltTab_ and _ShiftAltTab_ are two of the special commands that are only recognized when used on the same line as a hotkey. Here is the complete list:

**AltTab**: If the alt-tab menu is visible, move forward in it. Otherwise, display the menu (only if the hotkey is an &quot;&amp;&quot; combination of two keys; otherwise, it does nothing).

**ShiftAltTab**: Same as above except move backward in the menu.

**AltTabAndMenu**: If the alt-tab menu is visible, move forward in it. Otherwise, display the menu.

**AltTabMenuDismiss**: Close the Alt-tab menu.

To illustrate the above, the mouse wheel can be made into an entire substitute for Alt-tab. With the following hotkeys in effect, clicking the middle button displays the menu and turning the wheel navigates through it:

<pre>MButton::AltTabMenu
WheelDown::AltTab
WheelUp::ShiftAltTab</pre>

To cancel a hotkey-invoked Alt-tab menu without activating the selected window, use a hotkey such as the following. It might require adjustment depending on: 1) the means by which the alt-tab menu was originally displayed; and 2) whether the script has the [keyboard hook](commands/_InstallKeybdHook.htm) installed.

<pre>LCtrl &amp; CapsLock::AltTab
**!**MButton::  _; Middle mouse button. The **!** prefix makes it fire while the Alt key is down (which it is if the alt-tab menu is visible)._
IfWinExist ahk_class #32771  _; Indicates that the alt-tab menu is present on the screen._
    Send **!**{Escape}{Alt up}
return</pre>

Currently, all special Alt-tab actions must be assigned directly to a hotkey as in the examples above (i.e. they cannot be used as though they were commands). They are <span class="red">not affected by [#IfWin](commands/_IfWinActive.htm) or [#If](commands/_If.htm)</span>.

Custom alt-tab actions can also be created via hotkeys. In the following example, you would press F1 to display the menu and advance forward in it. Then you would press F2 to activate the selected window (or press Escape to cancel):

<pre>*F1::Send {Alt down}{tab} _; Asterisk is required in this case._
!F2::Send {Alt up}  _; Release the Alt key, which activates the selected window._
~*Escape::
IfWinExist ahk_class #32771
    Send {Escape}{Alt up}  _; Cancel the menu without activating the selected window._
return</pre>
</body>
</html>
