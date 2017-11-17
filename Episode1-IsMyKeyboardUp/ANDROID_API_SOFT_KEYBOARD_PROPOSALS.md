# New Keyboard is shown API _ideas_

## Option 1

```
InputMethodService input = context.getSystemService(Context.INPUT_METHOD_SERVICE);
boolean softKeyWillBeShown = input.addSoftKeyListener(new SoftKeyListener() {
   
 void didShowSoftKeyboard() {
   
 }
   
 void didHideSoftKeyboard() {

 }
    
});
```
_interface for new InputMethodService method addSoftKeyListener:_
```
/**
 * @return False if no software keyboard will be used and this listener is pointless
 */
boolean addSoftKeyListener(SoftKeyListner listener);
```

## Option 2 

```
InputMethodService input = context.getSystemService(Context.INPUT_METHOD_SERVICE);
if (input.willUseSoftwareKeyboard) {
  input.addSoftKeyListener(new SoftKeyLIstener() {
   
   void didShowSoftKeyboard() {
   
   }
   
   void didHideSoftKeyboard() {
   
   }
    
  });
}

```

## Option 3
```
InputMethodService input = context.getSystemService(Context.INPUT_METHOD_SERVICE);
input.addSoftKeyListener(new SoftKeyListener() {
   
 void possiblyShownKeyboard(boolean didShowSoftKey) {
   
 }
   
 void possiblyHiddenSoftKeyboard(boolean didHideSoftKey) {

 }
    
});
```
_interface for SoftKeyListener in this case:_
```
 /*
  * Called when the software keyboard is shown or would've been shown
  * but is not shown because of alternative input method being available.
  * @param didShowSoftKey true when software keyboard is actually shown.
  */
 void possiblyShownKeyboard(boolean didShowSoftKey);
 
 /*
  * Called when the software keyboard is hidden or would've been hideen
  * but is not hidden because of alternative input method being available.
  * @param didShowSoftKey true when software keyboard is actually hidden.
  */
 void possiblyHiddenSoftKeyboard(boolean didHideSoftKey);
```
