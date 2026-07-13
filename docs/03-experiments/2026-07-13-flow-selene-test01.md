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

## 結果（実行後に記入）
- 出力:
- 所要時間:
- 消費クレジット（Fast実測）:

## 評価（実行後に記入）
- キャラ一貫性（顔・服・画風の保持）:
- 「材料から動画」でのシーン合成の質:
- 歌声の質:
- 崩れ・問題点:

## 判定
（未実施）
