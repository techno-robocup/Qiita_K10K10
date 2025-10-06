---
title: cv2を使った赤・緑検知
tags:
  - Python
  - OpenCV
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

ライントレースをカメラで行うにあたり、赤色と緑色のマークを検知する必要がありました。  
今回は `cv2` を用いて色検知を行う方法をまとめます。  

---

# 色検知の基本

OpenCVでは色検知に **HSV色空間** を使うのが一般的です。  
OpenCVでのHSVでは値の範囲が次のようになっています。

- **H (色相)**: `0 ~ 179`  
- **S (彩度)**: `0 ~ 255`  
- **V (明度)**: `0 ~ 255`  

RGBと異なり、色相（Hue）を独立して扱えるため、明るさや照明条件の影響を受けにくくなります。  

HSV値の詳細は、[こちらの記事](https://qiita.com/rotarymars/items/84c49ebcd5972cbedf30)が参考になります。  

---

# モジュールのインポート

```python
import cv2
import numpy as np
```

# 赤・緑の閾値設定

今回の検知で使用した閾値は以下の通りです。

```python
lower_green = np.array([30, 40, 20])
upper_green = np.array([90, 255, 255])

lower_red = np.array([150, 130, 160])
upper_red = np.array([179, 255, 255])
```

# マスクの作成

画像をHSVに変換し、閾値を使ってマスクを作成します。

```python
hsv = cv2.cvtColor(orig_image, cv2.COLOR_RGB2HSV)

red_mask = cv2.inRange(hsv, lower_red, upper_red)
green_mask = cv2.inRange(hsv, lower_green, upper_green)
```

これで赤色と緑色の領域を抽出できます。

# ノイズ処理

実際の環境では小さなゴミやノイズを拾ってしまうことがあります。
そのため、最小面積を設定して小さい領域を無視したり、モルフォロジー処理（膨張・収縮）を追加すると安定します。

[コードの詳細は以下のリポジトリにまとめています。](https:github.com/techno-robocup/robocup2025_raspberrypi_library)

~Thank you for Reading~
