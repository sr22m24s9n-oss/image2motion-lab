# 2026-07-13 Flow テスト01 — セレネ「路地裏に逃れて、歌って踊る」

## 入力
- 使用画像: セレネ（GPT生成・権利問題なし）。Flow にキャラクター資産として登録済み
- 要望（日本語原文）: 「街中で、人目から逃れて路地裏に入る。そのあと、歌って踊る」

## 設計判断

- 動きが2拍構成（逃げ込む → 歌い踊る）なので **8秒クリップ×2本に分割**
- 登録画像は白背景の立ち絵のため、「フレームから動画」の開始フレームにすると
  白背景から始まる恐れあり → **「材料から動画」でキャラクター資産を材料に指定し、
  シーン（街・路地）はプロンプト側で記述**する方式を第一候補にする
- キャラの見た目はキャラクター資産が持つので、プロンプトでは服装・顔立ちを再説明しない
  （書くのは動き・カメラ・シーン・音だけ）

## クリップ1: 雑踏から路地裏へ（8秒）

```
Tracking shot at street level, following from behind. The girl weaves
through a crowded old-town market street, head lowered, glancing
nervously over her shoulder. She slips sideways into a narrow shadowed
back alley between tall stone buildings, presses her back to the wall,
and exhales in relief. Ambient sound: dense crowd chatter and footsteps,
fading and muffling as she enters the alley. Painterly dark fantasy
illustration style, muted earthy tones, soft overcast daylight.
```

対訳: 背後からの追従ショット。雑踏の旧市街を、俯きがちに肩越しを気にしながら縫うように歩き、
石造りの建物の間の狭い路地へ滑り込む。壁に背を預けて安堵の息。音は雑踏のざわめきが路地に入ると遠のく。

## クリップ2: 路地裏で歌って踊る（8秒）

```
Medium shot in a narrow stone back alley. The girl lifts her head,
begins to sing softly, then twirls and dances with light joyful steps,
her long tattered cloak flaring with each spin. The camera slowly
circles around her. Audio: a gentle melodic female singing voice
echoing off the alley walls, faint muffled city noise in the distance.
Painterly dark fantasy illustration style, a shaft of warm light
falling from above, dust motes drifting in the air.
```

対訳: 路地裏のミディアムショット。顔を上げて小さく歌い出し、くるりと回って軽やかに踊る。
マントが翻る。カメラはゆっくり周回。歌声が路地に反響し、遠くに街のくぐもった喧騒。

## クレジット節約の代替案（1本で済ませる場合）

タイムスタンプ式で1クリップに圧縮（駆け足になるのでテスト用）:

```
[00:00-00:04] Tracking shot: the girl slips out of a crowded street
into a narrow shadowed back alley, glancing over her shoulder.
[00:04-00:08] She turns, begins to sing, and twirls into a joyful
dance, her cloak flaring. Audio: crowd noise fading, then her soft
singing voice echoing in the alley. Painterly dark fantasy
illustration style.
```

## 結果（2026-07-13 実施）

- クリップ1: **生成拒否（有害判定）**
- クリップ2: 生成成功。歌付き（ただし英語）。消費 **15クレジット/本**
- 実行経路: エージェントチャット経由・デフォルト設定のまま
  （画像=Nano Banana 2、動画モデルはデフォルト。クリップのタイトルが英文
  "Girl singing and dancing in alle..." になっており、エージェントがテキストから組み立てた形跡）

## 評価

- **キャラ一貫性: 失敗。** 出力は幼い少女・青系の衣装・ディズニー風3DCGルックで、
  登録したキャラ（黒マント・長黒髪・厚塗りイラスト調）がほぼ反映されていない
- 歌声: 出る。言語は指定しないと英語になる
- 動き・音の同期自体は成立（歌って踊る内容は要望どおり）

## 原因分析

1. **クリップ1の有害判定**: 「少女を背後から追尾するカメラ + 人目を恐れる仕草」の
   組み合わせが追跡・逃走の構図と解釈された可能性が高い。
   → 恐怖・追尾の語を落とし「穏やかに路地へ入る」構図に書き換えて回避する
2. **キャラ・画風の不一致**: エージェント経由のデフォルト実行では、
   キャラクター資産が材料として使われず、テキストだけで生成された疑いが濃い。
   さらに動画モデルのデフォルトが Veo 3.1 ではない（設定画面では Omni Flash）。
   スタイル指定が弱いとディズニー風3DCGというモデルの「デフォルトの重力」に引っ張られる
3. 消費15/本はこの経路での実測値。Veo 3.1 Fast 直指定時の消費は別途要実測

## 修正方針（テスト02へ）

- 生成はエージェント任せにせず、**プロンプトボックスの「材料から動画」でセレネを明示添付**
- 設定（エージェントの設定 → 動画生成のデフォルト）で **モデルを Veo 3.1 Fast に変更**
- プロンプトに **2D厚塗りイラスト調の明示 + 3DCG否定を「望む状態」の言い方で**入れる
- 歌は **in Japanese** を明示

### クリップ1 修正版（安全フィルタ回避）

```
Wide establishing shot of a lively old-town market street at dusk.
A young woman in a long dark tattered cloak walks calmly along the
edge of the crowd, then turns into a quiet narrow back alley between
stone buildings and pauses with a small relieved smile. Ambient sound:
cheerful market chatter gently fading to quiet as she enters the alley.
Hand-painted 2D dark fantasy illustration style with visible brush
textures, muted earthy colors, soft overcast light.
```

### クリップ2 修正版（画風固定 + 日本語歌唱）

```
Medium shot in a narrow stone back alley. A young woman in a long
tattered dark cloak lifts her head, begins to sing softly in Japanese,
then twirls and dances with light joyful steps, her cloak flaring with
each spin. The camera slowly circles around her. Audio: a gentle
melodic female voice singing in Japanese, echoing off the stone walls.
Hand-painted 2D dark fantasy illustration style with visible brush
strokes, flat painterly shading, muted earthy colors, soft diffused
light falling from above.
```

## 判定

**条件付き（経路と設定を変えて再試行）** — 生成自体は通るが、
デフォルト経路ではキャラ資産と画風が反映されない。テスト02で
「材料から動画 + Veo 3.1 Fast + 画風明示」の三点を変えて比較する。

---

## テスト02 経過（同日・修正版クリップ2をエージェントに投入）

- **拒否されず生成に進んだ**。エージェントの思考プロセス表示:
  「a young woman, singing and dancing in a back alley, hand-painted 2D
  dark fantasy style, Japanese language が鍵。これから生成する」— 前回の
  有害判定とは別物で、正常進行のメッセージ
- 中間観測: 出力サムネイルは**2D厚塗り調に寄った**（画風ガードは効いた模様）。
  ただし衣装・顔はまだ登録キャラと不一致 → キャラ資産の添付が依然未解決
- 学び: 拒否か進行かは思考プロセス欄で判別できる。「young woman」は
  こちらのプロンプト由来の語であり、危険信号ではない
- 運用確定: 領主は英語が読めないため、**エージェントへの入力は日本語でよい**
  （エージェントはGemini基盤で日本語理解可）。パネルの英文はAI側が都度訳す分担

## テスト02 結果（同日）

- 出力: 約10秒クリップ「Woman singing and dancing in all...」。
  黒マント・黒髪の女性が石畳の路地で歌い踊る。**2D厚塗り調に矯正成功**
- 生成モデル: Omni Flash のまま（編集欄の表示から）。10秒/15クレジットと推定
- 評価:
  - 画風・衣装の系統・舞台: 要望にかなり接近。**画風はプロンプトで矯正できる**と確認
  - キャラ固有性: まだ別人（顔が整いすぎ・オッドアイなし・五芒星の飾りなし・
    髪や外套のボロボロ感が浅い・鞄なし）→ **プロンプト記述だけでは固有性は届かない**
- 導かれる仮説（分離則）: **画風はプロンプトで曲がるが、固有性は材料添付でしか固定できない**
- テスト03: 「材料から動画」でセレネのキャラ資産を明示添付し、同内容で生成して
  固有性が固定されるか検証。あわせてモデルを Veo 3.1 系に切り替えて比較
