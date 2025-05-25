---
title: Soothe Your Fingers - A Glorified Kanata & Ergonomic Keyboard Advertisement
date: 2025-05-25
tags: ["keyboard", "blog", "kanata"]
series: "Ergonomic Computing"
---

I’d like to consider myself something of a nerd. (Maybe even a *geek*, if you will.)

During the past few months, I found myself interested again in ergonomic keyboards, alternative keyboard layouts, and more recently home row modifications. At first, I started watching some episodes of Xah Lee’s *Xah Talk Show* focusing specifically on keyboard reviews, and more importantly their ergonomics. His knowledge on the subject is fairly extensive, and with the company of [his blog](https://xahlee.info), peering into the world of keyboards was a trivial task. Of course, I didn’t *only* look to Xah for keyboard knowledge. I also looked around on YouTube to hear what people had to say about the subject matter. Video format is possibly the best format to gain insight on physical items like keyboards, as the content creator can *show you* in real time what works and what doesn’t for a given product. After binging a bit too many videos, I came to the conclusion that I would buy an ergonomic keyboard (within my budget).

It was after visiting my local Office Depot that I settled on the [Logitech Wave Keys](https://www.logitech.com/en-hk/shop/p/wave-keys-ergonomic-wireless.920-012281) keyboard -- pictured below -- which was priced at a decent $60.

![Logitech Wave Keys in White, Black, and Pink](/images/wave_keys.png)

While it isn't a split ortholinear alternative-layout keyboard, I can still say it's worth the purchase for people wanting the most basic form of an ergonomic keyboard. The design of the Wave Keys seems to be geared towards reducing stress of the index fingers for reaching the keys in the middle of the keyboard. I found this to be a great change, as normal keyboards -- and especially laptop keyboards -- force the user to reach the middle keys in an uncomfortable manner. By creating a bump (or *wave* in this case) it becomes a breeze to access those keys, with the other four fingers on either hand already having decent access to the rest of the keyboard.

While the Wave Keys proved to be worth the purchase, I found myself wanting to experiment further, primarily with *programmable keyboards* and *key layering*.

You see, the keyboards that most are familiar with -- think the one built into your laptop, or the one sitting at your desk in the office -- are quite bare-bones in nature.  You can type on them, yes, but you are unable to determine which individual key sends which keystrokes. The Logitech Wave Keys suffers from this issue as well. You *are* able to utilize software to circumvent this issue, but this means that you are only able to use that configuration within the scope of the software running on *your* specific machine. Those shortcuts will not work on another machine without installing the software, which can be problematic in environments where the installation of software is prohibited (i.e. shared workplace, school computer).

**This is where programmable keyboards come in to save the day.**

Imagine owning a singular keyboard with firmware built in that allows you to configure and use your custom key bindings on *any computer*. Pretty crazy to imagine, but it exists and *has* existed for years. With this information at my disposal, I decided to buy *another* keyboard, which ended up being the [Attack Shark x AJAZZ AKS 068](https://attackshark.com/products/attackshark-ajazz-aks068-alicelayout-ergonomics?variant=48011973460266).

![Two color variants of the AKS 068 Pro](/images/IMG_0019.webp)

What puts the AKS 068 above the Wave Keys is the fact that it is similar to what is called an "Alice" keyboard. Alice-style keyboards tend to have a split between the middle keys, separating your hands and tilting them at an angle to create a more natural typing experience. While this does restrict the number of unique ways you can orient yourself for typing on the keyboard, it does help encourage healthier typing habits.

Aside from this, the ability to use [keyboard remapping software](https://caniusevia.com/) to configure the AKS 068 makes it a worthwhile purchase. The only issues with this keyboard in particular however, is that:

1. The keyboard is rather flexy in terms of its build, which may be a turn-off for those who prefer stronger build qualities.
1. From what I have seen, you are likely to have to use a [non-official JSON configuration file](https://github.com/xero/aks068-via) in order to *actually configure the thing.*
1. Even though you can remap keys, you are unable to utilize *mod-tap* functionality.
   The final issue I mentioned came as a surprise to me, as that was one of the features I bought the keyboard for.

To explain, a [modifier-tap key](https://docs.qmk.fm/mod_tap) functions as a normal keycode when pressed and released, and as a modifier (think of the shift, alt, control, and Windows keys on your keyboard) when held. This allows for something called [*home row modding*](https://precondition.github.io/home-row-mods), where the keys your fingers naturally rest on -- specifically the same row where the Caps Lock and Enter keys are -- act as modifiers when held. The benefit of this is that you no longer need to contort and cripple your hand for those hard to reach `Win + Ctrl + Shift + Alt` keys.

Since I couldn't program the AKS 068 to use home row mods, as well as the fact that my other keyboards weren't programmable at all, I decided to find a cross-platform application that I can bring across any of my devices using only a singular configuration file. While I did find programs like [AutoHotkey](https://www.autohotkey.com/) and [Microsoft PowerToys](https://learn.microsoft.com/en-us/windows/powertoys/), they proved to only support singular key remapping and not modifier layering. With AutoHotkey you *could* attempt to write a script to *imitate* that functionality, but my goal was to avoid hacky solutions.

That's when I found [Kanata](https://github.com/jtroo/kanata), the answer to all of my problems.

![Kanata Icon](/images/kanata-icon.svg)

Kanata is an open source, cross platform software keyboard remapper. It supports just about everything you could achieve with a keyboard sporting [QMK firmware](https://docs.qmk.fm/). It uses a lisp-like language for its configuration. For example, I'll show my current config and diagrams showing the different layers:

{{< details summary="See Code" >}}
````lisp
;; Kanata Config – CGAS home-row mods, Nav layer on Caps, Fn layer on Space
(defcfg
  process-unmapped-keys yes    ;; pass through keys not explicitly remapped
)

(defsrc
  ;; QWERTY top row (10 keys)
  q    w    e    r    t    y    u    i    o    p
  ;; Home row (10 keys: A through ; )
  a    s    d    f    g    h    j    k    l    scln
  ;; Additional keys for Nav/Fn layers
  n    m                 ;; bottom row keys used in Nav (N, M)
  caps spc             ;; CapsLock, Space
)

(defvar
  tap-time 250
  hold-time 200
)

(defalias
  ;; Home row dual-role keys: tap = letter, hold = modifier
  a      (tap-hold $tap-time $hold-time a    lctl)   ;; A -> Ctrl when held
  s      (tap-hold $tap-time $hold-time s    lmet)   ;; S -> GUI (Meta) when held
  d      (tap-hold $tap-time $hold-time d    lalt)   ;; D -> Alt when held
  f      (tap-hold $tap-time $hold-time f    lsft)   ;; F -> Shift when held
  j      (tap-hold $tap-time $hold-time j    rsft)   ;; J -> Right Shift when held
  k      (tap-hold $tap-time $hold-time k    ralt)   ;; K -> Right Alt when held
  l      (tap-hold $tap-time $hold-time l    rmet)   ;; L -> Right GUI (Meta) when held
  scln   (tap-hold $tap-time $hold-time scln rctl)   ;; ; -> Right Ctrl when held

  ;; CapsLock: tap = Esc, hold = momentary Nav layer
  navlayer (layer-while-held nav)                   ;; define Nav layer hold
  cap     (tap-hold $tap-time $hold-time esc  @navlayer)  ;; Caps -> Esc on tap, Nav layer on hold

  ;; Space: tap = space, hold = momentary Fn layer
  fnlayer (layer-while-held fn)                    ;; define Fn layer hold action
  space   (tap-hold $tap-time $hold-time spc  @fnlayer)   ;; Space -> Space on tap, Fn layer on hold
)

;; Base layer (default QWERTY) – use aliases for modified keys, transparent for others
(deflayer base
  _    _    _    _    _    _    _    _    _    _          ;; top row Q–P (unchanged)
  @a   @s   @d   @f   _    _    @j   @k   @l   @scln      ;; home row A–; (mods on A,S,D,F,J,K,L,;)
  _    _                                             ;; N, M (no remap)
  @cap @space                                        ;; CapsLock, Space (layer mods)
)

;; Nav layer (CapsLock held) – IJKL as arrows, U/O = Home/End, N/M = PgUp/PgDn
(deflayer nav
  _    _    _    _    _    _    home up   end  _         ;; U->Home, I->Up, O->End
  _    _    _    _    _    _    left down right _        ;; J->Left, K->Down, L->Right
  pgup pgdn                                          ;; N->PgUp, M->PgDn
  _    _                                       ;; Caps, Space do nothing on this layer
)

;; Fn layer (Space held) – Map selected keys to F1–F12, others pass through
(deflayer fn
  _    _    _    _    _    _    F9   F10  F11  F12       ;; U->F9, I->F10, O->F11, P->F12
  F1   F2   F3   F4   _    _    F5   F6   F7   F8        ;; A->F1 ... F->F4, J->F5 ... ;->F8
  _    _                                             ;; N, M not used (transparent)
  _    _                                           ;; Caps, Space (transparent in Fn layer)
)
````
{{< /details >}}

![Home Row Mods](/images/home_row_mods.jpg)
![Navigation Layer](/images/nav_layer.jpg)
![Function Key Layer](/images/fn_layer.jpg)

This setup enables home row mods on the `base` layer, makes a tap of Caps Lock send Escape and a hold activate navigation keys via the `nav` layer, and maps a held Spacebar to function keys on the `fn` layer.

Once you have a valid configuration, which you can [view documentation](https://jtroo.github.io/config.html) for and [conveniently test online](https://jtroo.github.io/), all you have to do is download/install the application and run the `kanata --cfg` command with your config file as an argument. If you run NixOS Linux, you can also [declaratively setup a Kanata systemd service](https://dev.to/shanu-kumawat/how-to-set-up-kanata-on-nixos-a-step-by-step-guide-1jkc) for maximum reproducibility across your devices.

Being able to use my ideal keyboard setup -- anywhere, on any machine -- is a game changer. Kanata doesn't just give me customization; it gives me *consistency*. And honestly, once you experience that kind of control, it’s hard to go back.

# References

* [Xah Lee's Blog](https://xahlee.info)
* [Logitech Wave Keys](https://www.logitech.com/en-hk/shop/p/wave-keys-ergonomic-wireless.920-012281)
* [Attack Shark x AJAZZ AKS 068](https://attackshark.com/products/attackshark-ajazz-aks068-alicelayout-ergonomics?variant=48011973460266)
* [VIA Keyboard Remapping Software](https://caniusevia.com/)
* [Xero's VIA JSON Files for the AKS 068 and AKS 068 Pro](https://github.com/xero/aks068-via)
* [QMK Documentation on Mod-Tap](https://docs.qmk.fm/mod_tap)
* [QMK Firmware Documentation](https://docs.qmk.fm/)
* [AutoHotkey](https://www.autohotkey.com/)
* [Microsoft PowerToys](https://learn.microsoft.com/en-us/windows/powertoys/)
* [Official Kanata GitHub Page](https://github.com/jtroo/kanata)
* [Kanata Configuration Guide](https://jtroo.github.io/config.html)
* [Kanata Simulator](https://jtroo.github.io/)
* [Guide to Setting Up Kanata on NixOS](https://dev.to/shanu-kumawat/how-to-set-up-kanata-on-nixos-a-step-by-step-guide-1jkc)
* [Guide to Home Row Mods](https://precondition.github.io/home-row-mods)
