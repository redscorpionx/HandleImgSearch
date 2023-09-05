# HandleImgSearch UDF

This repository contains a set of global image processing functions that can be used for various tasks, including capturing, searching, and comparing images. These functions provide flexibility and customization options to suit your specific needs.

## Table of Contents
- [GlobalImgInit](#globalimginit)
- [GlobalImgCapture](#globalimgcapture)
- [GlobalGetBitmap](#globalgetbitmap)
- [GlobalImgSearchRandom](#globalimgsearchrandom)
- [GlobalImgSearch](#globalimgsearch)
- [GlobalImgWaitExist](#globalimgwaitexist)
- [GlobalGetPixel](#globalgetpixel)
- [GlobalPixelCompare](#globalpixelcompare)
- [HandleImgSearch](#handleimgsearch)
- [HandleImgWaitExist](#handleimgwaitexist)
- [BmpImgSearch](#bmpimgsearch)
- [HandleGetPixel](#handlegetpixel)
- [HandlePixelCompare](#handlepixelcompare)
- [HandleCapture](#handlecapture)

## GlobalImgInit

### Description
Initializes global functions for image processing, allowing configuration of various parameters.

### Usage
```autoit
_GlobalImgInit($Hwnd, $X = 0, $Y = 0, $Width = -1, $Height = -1, $IsUser32 = False, $IsDebug = False, $Tolerance = 15, $Transparency = "", $MaxImg = 1000)
```

### Parameters
- `$Hwnd` (optional): Handle of the window.
- `$X`, `$Y`, `$Width`, `$Height` (optional): Area of the window that you want to capture. Default is the whole `$hwnd` window.
- `$IsUser32` (optional): Use `DllCall User32.dll` instead of `_WinAPI_BitBlt`. Default is False.
- `$IsDebug` (optional): Allows Debug mode. Default is False.
- `$Tolerance` (optional): Variation of the color (0 - 255). Default is 15.
- `$Transparency` (optional): The color in HEX that you want to set as transparency. Default is no transparency (e.g., "0xFFFFFF" to ignore all white color).
- `$MaxImg` (optional): Max image results that you want to find. Default is 1000.

## GlobalImgCapture

### Description
Captures a screenshot of the specified window or the entire screen if no window handle is provided.

### Usage
```autoit
_GlobalImgCapture([$Hwnd = 0])
```

### Parameters
- `$Hwnd`: Handle of the window if you don't use `_GlobalImgInit` to declare.

### Return Values
- `@error <> 0` if an error occurs.
- Returns Handle of the captured Bitmap.

## GlobalGetBitmap

### Description
Returns the handle of a bitmap image, which can be used for further image processing.

### Usage
```autoit
_GlobalGetBitmap()
```

### Return Values
- Returns the handle of the Bitmap.

## GlobalImgSearchRandom

### Description
Retrieves random coordinates of the first occurrence of a specified image.

### Usage
```autoit
_GlobalImgSearchRandom($BmpLocal, $IsReCapture = False, $BmpSource = 0, $IsRandom = True, $Tolerance = $_HandleImgSearch_Tolerance, $Transparency = $_HandleImgSearch_Transparency, $MaxImg = $_HandleImgSearch_MaxImg)
```

### Parameters
- `$BmpLocal`: The path of the BMP image to look for.
- `$IsReCapture` (optional): Capture again. Default is False.
- `$Tolerance` (optional): Variation of color. Default is 15.
- `$Transparency` (optional): The color in HEX that you want to set as transparency. Default is no transparency (e.g., "0xFFFFFF" to ignore all white color).
- `$BmpSource` (optional): Handle of the Bitmap if it's different from the Global value. Default is 0.
- `$IsRandom` (optional): Set to False if you don't want to randomize the result. Default is True.

### Return Values
- `@error = 1` if an error occurs.
- Returns random coordinates of the first result ($P[0] = $X, $P[1] = $Y).

## GlobalImgSearch

### Description
Searches for a given image within a specified window, providing information about the location and size of all occurrences found.

### Usage
```autoit
_GlobalImgSearch($BmpLocal, $IsReCapture = False, $BmpSource = 0, $Tolerance = $_HandleImgSearch_Tolerance, $Transparency = $_HandleImgSearch_Transparency, $MaxImg = $_HandleImgSearch_MaxImg)
```

### Parameters
- `$BmpLocal`: Path of the finding BMP image.
- `$IsReCapture` (optional): Re-capture the window. Default is False.
- `$BmpSource` (optional): Handle of Bitmap if not using Global.
- `$Tolerance` (optional): Variation of color.
- `$Transparency` (optional): The color in HEX that you want to set as transparency. Default is no transparency (e.g., "0xFFFFFF" to ignore all white color).
- `$MaxImg` (optional): Max number of results that you want to get.

### Return Values
- Success: Returns a 2D array with the following format:
  - `$aCords[0][0]` = Total results number
  - `$aCords[$i][0]` = X
  - `$aCords[$i][1]` = Y
  - `$aCords[$i][2]` = Width of bitmap
  - `$aCords[$i][3]` = Height of bitmap
- Error occurs: `@error <> 0`

## GlobalImgWaitExist

### Description
Waits for a specified image to appear within the declared handle or on the screen, with options for specifying a timeout and tolerance.

### Usage
```autoit
_GlobalImgWaitExist($BmpLocal, $TimeOutSecs = 5, $Tolerance = $_HandleImgSearch_Tolerance, $Transparency = $_HandleImgSearch_Transparency, $MaxImg = $_HandleImgSearch_MaxImg)
```

### Parameters
- `$BmpLocal`: The path of the BMP image to be searched.
- `$TimeOutSecs` (optional): The maximum time to search for the image in seconds. Default is 5.
- `$Tolerance` (optional): The color difference of the image.
- `$Transparency` (optional): The color in HEX that you want to set as transparency. Default is no transparency (e.g., "0xFFFFFF" to ignore all white color).
- `$MaxImg` (optional): The maximum number of search results returned.

### Return Values
- Success: Returns a 2D array with the following format:
  - `$aCords[0][0]` = Total number of positions found
  - `$aCords[$i][0]` = X coordinate
  - `$aCords[$i][1]` = Y coordinate
  - `$aCords[$i][2]` = Width of

 bitmap
  - `$aCords[$i][3]` = Height of bitmap
- Failure: Set `@error <> 0`

## GlobalGetPixel

### Description
Retrieves the color value of a specific pixel at given coordinates, optionally allowing re-capture of the image.

### Usage
```autoit
_GlobalGetPixel($X, $Y, $IsReCapture = False, $BmpSource = 0)
```

### Parameters
- `$X`, `$Y`: Coordinates for obtaining the color.
- `$IsReCapture` (optional): Capture again the image. Default is False.
- `$BmpSource` (optional): Bitmap handle when not using Global. Default is 0.

### Return Values
- Set `@error = 1` in case of an error.
- Return color code in the format `0xRRGGBB`.

## GlobalPixelCompare

### Description
Compares the color of a specific pixel with a provided color code, considering a specified tolerance level.

### Usage
```autoit
_GlobalPixelCompare($X, $Y, $PixelColor, $Tolerance = $_HandleImgSearch_Tolerance, $IsReCapture = False, $BmpSource = 0)
```

### Parameters
- `$X`, `$Y`: Coordinates to be compared.
- `$PixelColor`: Color to be compared.
- `$Tolerance` (optional): Tolerance value. Default is 20.
- `$IsReCapture` (optional): Retake the image. Default is False.
- `$BmpSource` (optional): Handle of the Bitmap if not using Global. Default is 0.

### Return Values
- Set `@error = 1` if an error occurs.
- Returns True if found, False if not found.

## HandleImgSearch

### Description
Searches for an image within a specified handle (window) or the entire screen.

### Usage
```autoit
_HandleImgSearch($hwnd, $bmpLocal, $x = 0, $y = 0, $iWidth = -1, $iHeight = -1, $Tolerance = 15, $Transparency = "", $MaxImg = 1000)
```

### Parameters
- `$hwnd`: Handle of the window to capture. If left blank, it will capture the desktop.
- `$bmpLocal`: Path to the BMP image to search for.
- `$x`, `$y`, `$iWidth`, `$iHeight`: Search area. Default is the entire image captured from `$hwnd`.
- `$Tolerance`: Color deviation of the image.
- `$Transparency` (optional): The color in HEX that you want to set as the transparency. Default is no transparency (e.g., "0xFFFFFF" to ignore all white color).
- `$MaxImg`: Maximum number of images to return.

### Return Values
- Success: Returns a 2D array with the following format:
  - `$aCords[0][0]` = Total number of positions found
  - `$aCords[$i][0]` = X coordinate
  - `$aCords[$i][1]` = Y coordinate
  - `$aCords[$i][2]` = Width of bitmap
  - `$aCords[$i][3]` = Height of bitmap
- Failure: Returns 0 and sets `@error` to 1

## HandleImgWaitExist

### Description
Waits for a specific image to appear within a specified handle (window) or on the screen, with options to specify a timeout, tolerance, and transparency.

### Usage
```autoit
_HandleImgWaitExist($hwnd, $bmpLocal, $timeOutSecs = 5, $x = 0, $y = 0, $iWidth = -1, $iHeight = -1, $Tolerance = 15, $Transparency = "", $MaxImg = 1000)
```

### Parameters
- `$hwnd`: Handle of the window to capture. If empty ("") the desktop will be captured.
- `$bmpLocal`: Path to the BMP image to search for.
- `$timeOutSecs`: Maximum search time (in seconds).
- `$x`, `$y`, `$iWidth`, `$iHeight`: Search region. By default, it searches the entire image captured from `$hwnd`.
- `$Tolerance`: Color deviation of the image.
- `$Transparency` (optional): The color in HEX that you want to set as the transparency. Default is no transparency (e.g., "0xFFFFFF" to ignore all white color).
- `$MaxImg`: Maximum number of images to return.

### Return Values
- Success: Returns a 2D array with the following format:
  - `$aCords[0][0]` = Total number of found positions
  - `$aCords[$i][0]` = X coordinate
  - `$aCords[$i][1]` = Y coordinate
  - `$aCords[$i][2]` = Width of the bitmap
  - `$aCords[$i][3]` = Height of the bitmap
- Failure: Returns 0 and sets `@error` to 1

## BmpImgSearch

### Description
Searches for an image within a BMP image.

### Usage
```autoit
_BmpImgSearch($SourceBmp, $FindBmp, $x = 0, $y = 0, $iWidth = -1, $iHeight = -1, $Tolerance = 15, $Transparency = "", $MaxImg = 1000)
```

### Parameters
- `$SourceBmp`: The path to the source BMP image.
- `$FindBmp`: The path to the BMP image to be searched for.
- `$x`, `$y`, `$iWidth`, `$iHeight`: The area to search in. The default is the entire image captured from `$hwnd`.
- `$Tolerance`: The color deviation of the image.
- `$Transparency` (optional): The color in HEX that you want to set as the transparency. Default is no transparency (e.g., "0xFFFFFF" to ignore all white color).
- `$MaxImg`: The maximum number of images to be returned.

### Return Values
- Success: Returns a 2D array with the following format:
  - `$aCords[0][0]` = The total number of locations found
  - `$aCords[$i][0]` = The X coordinate
  - `$aCords[$i][1]` = The Y coordinate
  - `$aCords[$i][2]` = The width of the bitmap
  - `$aCords[$i][3]` = The height of the bitmap
- Failure: Returns 0 and sets `@error` to 1

## HandleGetPixel

### Description
Retrieves the color value of a specific pixel within an image based on its handle.

### Usage
```autoit
_HandleGetPixel($hwnd, $getX, $getY, $x = 0, $y = 0, $Width = -1, $Height = -1)
```

### Parameters
- `$hwnd`: A handle value.
- `$getX`, `$getY`: The coordinates of the pixel to retrieve the color value.
-

 `$x`, `$y`, `$Width`, `$Height`: Optional parameters to specify the area of the image in the handle to capture. Default is the entire image captured from `$hwnd`.

### Return Values
- Sucess: Return the color in Hex (e.g., `0xFFFFFFFF`) 
- Failure: Return 0 and set `@error` to 1.

## HandlePixelCompare

### Description
Compares the color of a pixel within an image based on its handle with a provided color code, considering a specified tolerance level.

### Usage
```autoit
_HandlePixelCompare($hwnd, $getX, $getY, $pixelColor, $tolerance = 15, $x = 0, $y = 0, $Width = -1, $Height = -1)
```

### Parameters
- `$hwnd`: A handle value of the image.
- `$getX`, `$getY`: The coordinates of the pixel to compare.
- `$pixelColor`: The color to compare to in hex format (e.g., "0xFF0000" for red).
- `$tolerance`: The color tolerance value in range 0-255.
- `$x`, `$y`, `$Width`, `$Height`: Optional parameters to specify the area of the image in the handle to capture. Default is the entire image captured from `$hwnd`.

### Return Values
- Returns True if the color of the pixel matches within the specified tolerance, False otherwise.
- Returns `@error = 1` if there is an error in capturing the image or getting the pixel color.

## HandleCapture

### Description
Captures an image of a specified window or the entire screen based on its handle, providing options to customize the capture area and image format.

### Usage
```autoit
_HandleCapture($hwnd, $x = 0, $y = 0, $Width = -1, $Height = -1, $IsBMP = False, $SavePath = "", $IsUser32 = False)
```

### Parameters
- `$hwnd` (optional): Handle of the window to capture. If empty ("") then the function captures the current screen.
- `$x` (optional): The x-coordinate of the upper-left corner of the image in the window. Default is 0.
- `$y` (optional): The y-coordinate of the upper-left corner of the image in the window. Default is 0.
- `$Width` (optional): The width of the image in the window. Default is -1 (the entire window is captured).
- `$Height` (optional): The height of the image in the window. Default is -1 (the entire window is captured).
- `$IsBMP` (optional): True if the function returns a bitmap. False if the function returns an HBITMAP. Default is False.
- `$SavePath` (optional): The file path to save the captured image. Default is an empty string.
- `$IsUser32` (optional): True to use User32.dll instead of `_WinAPI_BitBlt`. Default is False (try to get the right result).

### Return Values
- Success: Returns a 2D array with the following format:
  - `$aCords[0][0]` = Total number of found positions
  - `$aCords[$i][0]` = X coordinate
  - `$aCords[$i][1]` = Y coordinate
  - `$aCords[$i][2]` = Width of the bitmap
  - `$aCords[$i][3]` = Height of the bitmap
- Failure: Returns 0 and sets `@error` to 1
