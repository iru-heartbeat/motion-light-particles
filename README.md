# Motion Light Particles

Webカメラの前で手を振るだけでパーティクルが反応する、非接触型のインタラクティブなビジュアル体験です。カメラを利用できない場合はマウス操作にフォールバックします。

詳細な要件は [`要件定義書.md`](./要件定義書.md) を参照してください。

## デモ

https://motion-light-particles.vercel.app/

## 技術スタック

- **言語**: HTML5 / Vanilla JavaScript（フレームワーク・ビルドツール不使用、単一HTMLファイル）
- **描画**: Canvas API（`CanvasRenderingContext2D`）
- **カメラ入力**: `navigator.mediaDevices.getUserMedia`
- **モーション検知**: 前フレームとの画素差分（グレースケール変換＋グリッド単位の差分比較）を独自実装
- **ホスティング / デプロイ**: Vercel（GitHub連携による自動デプロイ、`vercel.json`でルートパスの配信先を設定）
- **バージョン管理**: GitHub

## 主な機能

- カメラでの動き検知に反応するパーティクル発生・減衰
- カメラ許可拒否時のマウス操作フォールバック
- 感度調整スライダー（環境の明るさに応じて検知しきい値を調整）

## ローカルでの確認方法

ビルド不要です。`motion_light_particles.html` をブラウザで直接開いてください。
