# kisses
Keep It Simple Stupid Emacs Splash

## Motivation

I wanted a really simple splash screen that simply centered a nice banner. But I wanted this splash screen to behave nicely:

* If a window gets resized while the splash screen is either focused or unfocused, the banner should still be centered in the newly resized window. 
* If the splash buffer is displayed in two different windows that are different sizes, the banner should be centered in both windows. 

Keep it simple stupid, right? 

## Usage

There are two main functions (both idempotent) and one main variable. 

* `kisses-redraw` which creates a buffer called *splash* and activates `kisses-mode` in that buffer then centers the banner.
* `kisses-recenter` which just recenter the banner. 
* `kisses-banner` which simply stores the banner text (hint, you can add text properties to the text to format the banner) 

### Configuration to avoid calling the above functions yourself: 

```elisp
(use-package kisses
  :straight (kisses :type git :host github :repo "jsilve24/kisses")
  :ensure t
  :config
  ;; I just use the default banner and add some nice text property to it
  (put-text-property 0 (length kisses-banner) 'face 'outline-1
		     kisses-banner)
  ;; This makes it a nice initial buffer (not the default just a nice fallback)
  (setq initial-buffer-choice 'kisses-initial-buffer))
```


## How it works

This package creates one large buffer of size `kisses--max-rows` by `kisses--max-columns` and places the `kisses-banner` in the center. Then each window displaying the `*splash*` buffer just has to scroll the window so that the banner is centered. This ensures that multiple windows can display the banner as centered without having to create a bunch of duplicate copies of the buffer. 

I would bet there is a more elegant way of doing this but my elisp is relatively weak and this is what I came up with on the spot. 

