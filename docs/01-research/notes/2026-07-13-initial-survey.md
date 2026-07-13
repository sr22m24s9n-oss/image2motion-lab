# 2026-07-13 初期調査 — 無料 image-to-video の現状概観

Web検索による一次スイープ。個別の裏取り（各サービスに実際に登録して無料枠を確認）は未実施。
**無料枠の内容は数週間単位で変わるため、テスト直前に必ず再確認すること。**

## 大前提（調査で確定した事実）

- 「完全無料・無制限・透かしなし」は存在しない。無料の実態は
  **(a) 毎日/毎月リセットされる無料クレジット** か **(b) 透かし付き・低解像度** のどちらか。
- 商用利用可否はサービスごとに割れる。無料枠は「透かし付き・商用不可」が多い。
  → 知人の用途（SNS投稿か個人鑑賞か）の確認が必須（02-requirements 参照）。
- ローカル生成（Stable Video Diffusion 等）は透かしなし・無制限だが、
  本環境（Intel HD 620 / GPUなし）では不可。代替は Google Colab 無料枠。

## ルートA: Webサービス無料枠（本命）

| 候補 | 無料枠（2026年時点の報告値） | 透かし | アニメ適性 | 所感 |
| --- | --- | --- | --- | --- |
| Kling | 毎日リセット 約66クレジット/日 — 無料枠は業界最大級 | あり | 高品質だが実写寄り評価 | 毎日枠がある点で継続運用向き。最有力候補の一つ |
| PixVerse | 登録時90 + 毎日60クレジット（≒1本/日） | あり・低解像度 | **アニメ・スタイライズに強いと評判** | アニメ系素材との相性で第一候補 |
| Vidu | 月次の少量クレジット + **オフピーク時無制限モード** | あり | アニメ系に定評 | オフピーク無制限は無料運用と好相性 |
| Canva | 標準モデル 月200回 | 要確認 | 要確認 | 編集（カット・BGM）まで一括でできるのが利点 |
| Hailuo | 無料枠あり（詳細要確認） | あり | 人物モーション向き | 補欠 |
| Luma | 月数回程度の少量 | あり | 要確認 | 枠が小さく本命にしづらい |
| Google Veo (Gemini) | 無料枠でクリーンな出力の報告あり | 要確認 | 実写寄り | 品質は高い。枠の実態要確認 |

## ルートB: Google Colab 無料枠 + オープンモデル（保険）

- AnimateDiff / CogVideoX 等の公開ノートブックが存在し、無料T4で動作報告あり。
- 制約: 512×512×24フレーム程度が現実線、1クリップ生成に2〜15分、
  セッション切断（30〜90分アイドル）、混雑時GPU割当なし。
- 透かしなし・回数実質無制限（GPU割当の範囲内）だが、品質・手間はルートAに劣る。
- 位置づけ: **ルートAの無料枠を使い切った場合・透かしNGの場合の代替**。

## 次のアクション

1. 知人への確認事項（02-requirements）の回答待ち。特に「用途（透かし許容度）」と「動きの温度感」
2. テスト素材（自作 or フリーのアニメ系イラスト）を1枚確定
3. PixVerse → Kling → Vidu の順に実登録して無料枠の実態を確認、同一素材でテスト生成（03-experiments に記録）
4. Colabノートブック（camenduru/AnimateDiff-colab 等）の生存確認

## 出典

- https://aipicks.jp/mag/image-to-video-ai-free-2026
- https://aipicks.jp/mag/free-ai-video-generation-2026
- https://ai-kenkyujo.com/software/generative-ai/gazou-kara-douga-seiseiai-muryou/
- https://miralab.co.jp/media/best_ai_video_generators/
- https://whichoneisreal.com/compare/best-free-ai-video/
- https://www.atlascloud.ai/blog/guides/pixverse-ai-free
- https://aiimagetovideo.pro/blog/free-kling-ai-video-generator/
- https://morphed.app/blog/free-ai-video-generators-no-watermark
- https://ponpon.ai/blog/best-free-ai-image-to-video-tools-2026
- https://www.hivenet.com/post/google-colaboratory-gpu-complete-guide-to-free-cloud-gpu-access-and-limitations
- https://education.civitai.com/beginners-guide-to-animatediff/
- https://colab.research.google.com/github/camenduru/AnimateDiff-colab/blob/main/AnimateDiff_colab.ipynb
