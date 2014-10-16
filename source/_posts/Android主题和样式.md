title: Android主题和样式
date: 2014-10-14 13:14:42
categories: Android
tags: [Holo]
---
<!--more-->
##位置
在Android Frameworks/base/core/res/res/values目录下
- `themes.xml`
- `themes_device_defaults.xml`
- `styles.xml`
- `styles_device_defaults.xml`

##主题Theme
`theme.xml`定义了Android低版本的theme和Holo theme，`themes_device_defaults.xml`定义了DeviceDefault主题(继承自Holo主题)
系统如何选择默认主题呢？
```java
/**frameworks/base/core/java/android/content/res/Resources.java*/
public static int selectDefaultTheme(int curTheme, int targetSdkVersion) {
    return selectSystemTheme(curTheme, targetSdkVersion,
            com.android.internal.R.style.Theme,
            com.android.internal.R.style.Theme_Holo,
            com.android.internal.R.style.Theme_DeviceDefault);
}    

public static int selectSystemTheme(int curTheme, int targetSdkVersion,
        int orig, int holo, int deviceDefault) {
    if (curTheme != 0) { 
        return curTheme;
    }    
    if (targetSdkVersion < Build.VERSION_CODES.HONEYCOMB) {
        // < 11
        return orig;
    }    
    if (targetSdkVersion < Build.VERSION_CODES.ICE_CREAM_SANDWICH) {
         // < 14
         return holo;
    }    
    return deviceDefault;
}
```
当<11时使用低版本主题，>=11&&<14时使用Holo主题，>14时，使用DeviceDefault主题

##系统主题Theme列表
系统默认大的主题是三种：Theme,Theme.Holo,Theme.DeviceDefault, 但是实际上在此基础系统还定义了大量的派生主题，最典型的是对应的Light主题。
![](/img/14101601.png)

##详解每个主题item分类
1. 颜色
```xml
<item name="colorForeground">@android:color/bright_foreground_dark</item>
<item name="colorForegroundInverse">@android:color/bright_foreground_dark_inverse</item>
<item name="colorBackground">@android:color/background_dark</item>
<item name="colorBackgroundCacheHint">?android:attr/colorBackground</item>
<item name="colorPressedHighlight">@color/legacy_pressed_highlight</item>
<item name="colorLongPressedHighlight">@color/legacy_long_pressed_highlight</item>
<item name="colorFocusedHighlight">@color/legacy_selected_highlight</item>
<item name="colorMultiSelectHighlight">@color/legacy_selected_highlight</item>
<item name="colorActivatedHighlight">@color/legacy_selected_highlight</item>
```
2. 字体
```xml
<item name="textAppearance">@android:style/TextAppearance</item>
<item name="textAppearanceInverse">@android:style/TextAppearance.Inverse</item>
 
<item name="textColorPrimary">@android:color/primary_text_dark</item>
<item name="textColorSecondary">@android:color/secondary_text_dark</item>
<item name="textColorTertiary">@android:color/tertiary_text_dark</item>
<item name="textColorPrimaryInverse">@android:color/primary_text_light</item>
<item name="textColorSecondaryInverse">@android:color/secondary_text_light</item>
<item name="textColorTertiaryInverse">@android:color/tertiary_text_light</item>
<item name="textColorPrimaryDisableOnly">@android:color/primary_text_dark_disable_only</item>
<item name="textColorPrimaryInverseDisableOnly">@android:color/primary_text_light_disable_only</item>
<item name="textColorPrimaryNoDisable">@android:color/primary_text_dark_nodisable</item>
<item name="textColorSecondaryNoDisable">@android:color/secondary_text_dark_nodisable</item>
<item name="textColorPrimaryInverseNoDisable">@android:color/primary_text_light_nodisable</item>
<item name="textColorSecondaryInverseNoDisable">@android:color/secondary_text_light_nodisable</item>
<item name="textColorHint">@android:color/hint_foreground_dark</item>
<item name="textColorHintInverse">@android:color/hint_foreground_light</item>
<item name="textColorSearchUrl">@android:color/search_url_text</item>
<item name="textColorHighlight">@android:color/highlighted_text_dark</item>
<item name="textColorHighlightInverse">@android:color/highlighted_text_light</item>
<item name="textColorLink">@android:color/link_text_dark</item>
<item name="textColorLinkInverse">@android:color/link_text_light</item>
<item name="textColorAlertDialogListItem">@android:color/primary_text_light_disable_only</item>
 
<item name="textAppearanceLarge">@android:style/TextAppearance.Large</item>
<item name="textAppearanceMedium">@android:style/TextAppearance.Medium</item>
<item name="textAppearanceSmall">@android:style/TextAppearance.Small</item>
<item name="textAppearanceLargeInverse">@android:style/TextAppearance.Large.Inverse</item>
<item name="textAppearanceMediumInverse">@android:style/TextAppearance.Medium.Inverse</item>
<item name="textAppearanceSmallInverse">@android:style/TextAppearance.Small.Inverse</item>
<item name="textAppearanceSearchResultTitle">@android:style/TextAppearance.SearchResult.Title</item>
<item name="textAppearanceSearchResultSubtitle">@android:style/TextAppearance.SearchResult.Subtitle</item>
 
<item name="textAppearanceEasyCorrectSuggestion">@android:style/TextAppearance.EasyCorrectSuggestion</item>
<item name="textAppearanceMisspelledSuggestion">@android:style/TextAppearance.MisspelledSuggestion</item>
<item name="textAppearanceAutoCorrectionSuggestion">@android:style/TextAppearance.AutoCorrectionSuggestion</item>
 
<item name="textAppearanceButton">@android:style/TextAppearance.Widget.Button</item>
 
<item name="editTextColor">@android:color/primary_text_light</item>
<item name="editTextBackground">@android:drawable/edit_text</item>
 
<item name="candidatesTextStyleSpans">@android:string/candidates_style</item>
 
<item name="textCheckMark">@android:drawable/indicator_check_mark_dark</item>
<item name="textCheckMarkInverse">@android:drawable/indicator_check_mark_light</item>
 
<item name="textAppearanceLargePopupMenu">@android:style/TextAppearance.Widget.PopupMenu.Large</item>
<item name="textAppearanceSmallPopupMenu">@android:style/TextAppearance.Widget.PopupMenu.Small</item>
```
3. 按钮
```xml
<item name="buttonStyle">@android:style/Widget.Button</item>
 
<item name="buttonStyleSmall">@android:style/Widget.Button.Small</item>
<item name="buttonStyleInset">@android:style/Widget.Button.Inset</item>
 
<item name="buttonStyleToggle">@android:style/Widget.Button.Toggle</item>
 
<item name="selectableItemBackground">@android:drawable/item_background</item>
<item name="borderlessButtonStyle">?android:attr/buttonStyle</item>
<item name="homeAsUpIndicator">@android:drawable/ic_ab_back_holo_dark</item>
```
4. List
```xml
<item name="listPreferredItemHeight">64dip</item>
<item name="listPreferredItemHeightSmall">?android:attr/listPreferredItemHeight</item>
<item name="listPreferredItemHeightLarge">?android:attr/listPreferredItemHeight</item>
<item name="dropdownListPreferredItemHeight">?android:attr/listPreferredItemHeight</item>
<item name="textAppearanceListItem">?android:attr/textAppearanceLarge</item>
<item name="textAppearanceListItemSmall">?android:attr/textAppearanceLarge</item>
<item name="listPreferredItemPaddingLeft">6dip</item>
<item name="listPreferredItemPaddingRight">6dip</item>
<item name="listPreferredItemPaddingStart">6dip</item>
<item name="listPreferredItemPaddingEnd">6dip</item>
```
5. Window
```xml
<item name="windowBackground">@android:drawable/screen_background_selector_dark</item>
<item name="windowFrame">@null</item>
<item name="windowNoTitle">false</item>
<item name="windowFullscreen">false</item>
<item name="windowOverscan">false</item>
<item name="windowIsFloating">false</item>
<item name="windowContentOverlay">@null</item>
<item name="windowShowWallpaper">false</item>
<item name="windowTitleStyle">@android:style/WindowTitle</item>
<item name="windowTitleSize">25dip</item>
<item name="windowTitleBackgroundStyle">@android:style/WindowTitleBackground</item>
<item name="android:windowAnimationStyle">@android:style/Animation.Activity</item>
<item name="android:windowSoftInputMode">stateUnspecified|adjustUnspecified</item>
<item name="windowActionBar">false</item>
<item name="windowActionModeOverlay">false</item>
<item name="windowCloseOnTouchOutside">false</item>
<item name="windowTranslucentStatus">false</item>
<item name="windowTranslucentNavigation">false</item>
```
6. Dialog
```xml
<item name="dialogTheme">@android:style/Theme.Dialog</item>
<item name="dialogTitleIconsDecorLayout">@layout/dialog_title_icons</item>
<item name="dialogCustomTitleDecorLayout">@layout/dialog_custom_title</item>
<item name="dialogTitleDecorLayout">@layout/dialog_title</item>
```
7. AlertDialog
```xml
<item name="alertDialogTheme">@android:style/Theme.Dialog.Alert</item>
<item name="alertDialogStyle">@android:style/AlertDialog</item>
<item name="alertDialogCenterButtons">true</item>
<item name="alertDialogIcon">@android:drawable/ic_dialog_alert</item>
```
8. Panel
```xml
<item name="panelBackground">@android:drawable/menu_background</item>
<item name="panelFullBackground">@android:drawable/menu_background_fill_parent_width</item>
<!-- These three attributes do not seems to be used by the framework. Declared public though -->
<item name="panelColorBackground">#000</item>
<item name="panelColorForeground">?android:attr/textColorPrimary</item>
<item name="panelTextAppearance">?android:attr/textAppearance</item>
 
<item name="panelMenuIsCompact">false</item>
<item name="panelMenuListWidth">296dip</item>
```
9. 滚动条(Scrollbar)
```xml
<item name="scrollbarFadeDuration">250</item>
<item name="scrollbarDefaultDelayBeforeFade">300</item> 
<item name="scrollbarSize">10dip</item>
<item name="scrollbarThumbHorizontal">@android:drawable/scrollbar_handle_horizontal</item>
<item name="scrollbarThumbVertical">@android:drawable/scrollbar_handle_vertical</item>
<item name="scrollbarTrackHorizontal">@null</item>
<item name="scrollbarTrackVertical">@null</item>
```
10. 文字选中(Text selection)
```xml
<item name="textSelectHandleLeft">@android:drawable/text_select_handle_left</item>
<item name="textSelectHandleRight">@android:drawable/text_select_handle_right</item>
<item name="textSelectHandle">@android:drawable/text_select_handle_middle</item>
<item name="textSelectHandleWindowStyle">@android:style/Widget.TextSelectHandle</item>
<item name="textEditPasteWindowLayout">@android:layout/text_edit_paste_window</item>
<item name="textEditNoPasteWindowLayout">@android:layout/text_edit_no_paste_window</item>
<item name="textEditSidePasteWindowLayout">@android:layout/text_edit_side_paste_window</item>
<item name="textEditSideNoPasteWindowLayout">@android:layout/text_edit_side_no_paste_window</item>
<item name="textSuggestionsWindowStyle">@android:style/Widget.TextSuggestionsPopupWindow</item>
<item name="textEditSuggestionItemLayout">@android:layout/text_edit_suggestion_item</item>
```