<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JLPT N5 Japanese Verb Quiz & 100-Verb Reference</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts & Icons -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Noto+Sans+JP:wght@400;500;700&family=Shippori+Mincho:wght@500;600;700&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/d3@7.9.0"></script>
    <style>
        :root {
            --sakura: #d9829b;
            --sakura-deep: #a84f6a;
            --sakura-soft: #f8e8ed;
            --ume: #8f3f59;
            --matcha: #71836b;
            --matcha-soft: #edf2e9;
            --ink: #3f3540;
            --paper: #fffaf7;
            --paper-deep: #f7eee9;
            --gold: #c5a46d;
        }

        body {
            font-family: 'Inter', 'Noto Sans JP', sans-serif;
            color: var(--ink);
            background:
                radial-gradient(circle at 8% 12%, rgba(217,130,155,.12) 0 2px, transparent 3px),
                radial-gradient(circle at 92% 30%, rgba(197,164,109,.10) 0 2px, transparent 3px),
                linear-gradient(145deg, #fffdfb 0%, #fff8f6 48%, #f8f3ef 100%);
            background-size: 46px 46px, 58px 58px, auto;
        }

        body::before,
        body::after {
            content: "✿";
            position: fixed;
            z-index: -1;
            color: rgba(217,130,155,.10);
            font-family: serif;
            line-height: 1;
            pointer-events: none;
        }

        body::before {
            top: 88px;
            left: -22px;
            font-size: 150px;
            transform: rotate(-15deg);
        }

        body::after {
            right: -28px;
            bottom: 20px;
            font-size: 180px;
            transform: rotate(18deg);
        }

        h1, h2, h3, .japanese-heading {
            font-family: 'Shippori Mincho', 'Noto Sans JP', serif;
            letter-spacing: .025em;
        }

        .japanese-text {
            font-family: 'Noto Sans JP', sans-serif;
        }

        ruby rt {
            color: var(--sakura-deep);
            font-size: .48em;
            font-weight: 600;
        }

        header {
            position: relative;
            overflow: hidden;
            background:
                linear-gradient(110deg, rgba(143,63,89,.97), rgba(168,79,106,.95)),
                repeating-linear-gradient(45deg, transparent 0 12px, rgba(255,255,255,.035) 12px 13px);
            border-bottom: 3px solid rgba(197,164,109,.85);
        }

        header::after {
            content: "桜  •  日本語  •  学び";
            position: absolute;
            right: 2rem;
            bottom: .35rem;
            color: rgba(255,255,255,.13);
            font-family: 'Shippori Mincho', serif;
            font-size: 1.15rem;
            letter-spacing: .45em;
            pointer-events: none;
        }

        #quiz-card,
        #quiz-view > div,
        #table-view > div {
            border-color: rgba(217,130,155,.24) !important;
            box-shadow: 0 18px 45px rgba(99,61,76,.08) !important;
        }

        #quiz-card {
            position: relative;
            background: rgba(255,250,247,.96) !important;
            border-top: 5px solid var(--sakura) !important;
        }

        #quiz-card::before {
            content: "❀";
            position: absolute;
            right: 1.25rem;
            top: .9rem;
            color: rgba(217,130,155,.25);
            font-size: 2rem;
        }

        button {
            letter-spacing: .01em;
        }

        button[class*="bg-indigo"],
        #btn-next-question,
        #btn-typed-submit {
            background: linear-gradient(135deg, var(--sakura-deep), var(--sakura)) !important;
            border-color: transparent !important;
            box-shadow: 0 7px 16px rgba(168,79,106,.18);
        }

        button[class*="bg-indigo"]:hover,
        #btn-next-question:hover,
        #btn-typed-submit:hover {
            background: linear-gradient(135deg, #91435d, #ca718b) !important;
            transform: translateY(-1px);
        }

        #btn-quiz-mode,
        #btn-table-mode {
            border: 1px solid rgba(255,255,255,.28) !important;
            backdrop-filter: blur(5px);
        }

        #quiz-badge-type {
            background: var(--sakura-soft) !important;
            color: var(--sakura-deep) !important;
            border: 1px solid rgba(217,130,155,.25);
            border-radius: 999px !important;
            padding-inline: .7rem !important;
        }

        #quiz-progress-bar {
            background: linear-gradient(90deg, var(--sakura-deep), #e8a7b9) !important;
        }

        #quiz-options-container button {
            background: rgba(255,255,255,.88) !important;
            border-color: rgba(168,79,106,.17) !important;
            border-radius: 1rem !important;
        }

        #quiz-options-container button:hover {
            background: #fff7f9 !important;
            border-color: var(--sakura) !important;
            box-shadow: 0 8px 18px rgba(168,79,106,.08);
            transform: translateY(-1px);
        }

        input:focus {
            --tw-ring-color: rgba(217,130,155,.52) !important;
            border-color: var(--sakura) !important;
        }

        thead {
            background: linear-gradient(90deg, #f9e9ee, #f3eee8) !important;
            color: var(--ume) !important;
        }

        tbody tr:hover {
            background: rgba(248,232,237,.42) !important;
        }

        .bg-indigo-50 {
            background-color: var(--sakura-soft) !important;
        }

        .text-indigo-600,
        .text-indigo-700 {
            color: var(--sakura-deep) !important;
        }

        .border-indigo-100,
        .border-indigo-200,
        .border-indigo-300 {
            border-color: rgba(217,130,155,.30) !important;
        }

        .bg-slate-50 {
            background-color: rgba(255,250,247,.88) !important;
        }

        .bg-slate-100 {
            background-color: var(--paper-deep) !important;
        }

        .text-slate-800,
        .text-slate-900 {
            color: var(--ink) !important;
        }

        ::selection {
            background: #f0c7d3;
            color: #563342;
        }

        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }

        ::-webkit-scrollbar-track {
            background: #f6ece8;
            border-radius: 999px;
        }

        ::-webkit-scrollbar-thumb {
            background: #d9a5b4;
            border-radius: 999px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: #bf7f93;
        }

        @media (max-width: 700px) {
            header::after {
                display: none;
            }
            body::before,
            body::after {
                opacity: .55;
            }
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen flex flex-col relative">

<script>
// Curated database of 100 JLPT N5 verb expressions.
// Includes common する-verbs and fixed expressions such as 授業を受ける.
const N5_VERBS = [
    {
        "id": 1,
        "display": "浴びる",
        "dictionary": "あびる",
        "romaji": "abiru",
        "meaning": "To bathe / take a shower",
        "masu": "あびます",
        "particle": "を",
        "sentence": "まいあさ、シャワーを浴びます。",
        "isN5Kanji": true,
        "te": "あびて"
    },
    {
        "id": 2,
        "display": "上げる",
        "dictionary": "あげる",
        "romaji": "ageru",
        "meaning": "To raise / give",
        "masu": "あげます",
        "particle": "を/に",
        "sentence": "ともだちにプレゼントを上げます。",
        "isN5Kanji": true,
        "te": "あげて"
    },
    {
        "id": 3,
        "display": "開ける",
        "dictionary": "あける",
        "romaji": "akeru",
        "meaning": "To open (something)",
        "masu": "あけます",
        "particle": "を",
        "sentence": "まどを開けます。",
        "isN5Kanji": true,
        "te": "あけて"
    },
    {
        "id": 4,
        "display": "開く",
        "dictionary": "あく",
        "romaji": "aku",
        "meaning": "To open (intransitive)",
        "masu": "あきます",
        "particle": "が",
        "sentence": "ドアが開きます。",
        "isN5Kanji": true,
        "te": "あいて"
    },
    {
        "id": 5,
        "display": "洗う",
        "dictionary": "あらう",
        "romaji": "arau",
        "meaning": "To wash",
        "masu": "あらいます",
        "particle": "を",
        "sentence": "てを洗います。",
        "isN5Kanji": true,
        "te": "あらって"
    },
    {
        "id": 6,
        "display": "ある",
        "dictionary": "ある",
        "romaji": "aru",
        "meaning": "To exist / have (inanimate)",
        "masu": "あります",
        "particle": "が/に",
        "sentence": "つくえのうえにほんがあります。",
        "isN5Kanji": true,
        "te": "あって"
    },
    {
        "id": 7,
        "display": "歩く",
        "dictionary": "あるく",
        "romaji": "aruku",
        "meaning": "To walk",
        "masu": "あるきます",
        "particle": "を/まで",
        "sentence": "えきまで歩きます。",
        "isN5Kanji": true,
        "te": "あるいて"
    },
    {
        "id": 8,
        "display": "遊ぶ",
        "dictionary": "あそぶ",
        "romaji": "asobu",
        "meaning": "To play / enjoy oneself",
        "masu": "あそびます",
        "particle": "で/と",
        "sentence": "こうえんでともだちと遊びます。",
        "isN5Kanji": true,
        "te": "あそんで"
    },
    {
        "id": 9,
        "display": "会う",
        "dictionary": "あう",
        "romaji": "au",
        "meaning": "To meet",
        "masu": "あいます",
        "particle": "に/と",
        "sentence": "えきでともだちに会います。",
        "isN5Kanji": true,
        "te": "あって"
    },
    {
        "id": 10,
        "display": "勉強する",
        "dictionary": "べんきょうする",
        "romaji": "benkyou suru",
        "meaning": "To study",
        "masu": "べんきょうします",
        "particle": "を",
        "sentence": "まいにち、にほんごを勉強します。",
        "isN5Kanji": true,
        "te": "べんきょうして"
    },
    {
        "id": 11,
        "display": "違う",
        "dictionary": "ちがう",
        "romaji": "chigau",
        "meaning": "To differ / be wrong",
        "masu": "ちがいます",
        "particle": "と/が",
        "sentence": "このこたえは違います。",
        "isN5Kanji": true,
        "te": "ちがって"
    },
    {
        "id": 12,
        "display": "出す",
        "dictionary": "だす",
        "romaji": "dasu",
        "meaning": "To take out / submit / send",
        "masu": "だします",
        "particle": "を",
        "sentence": "しゅくだいを出します。",
        "isN5Kanji": true,
        "te": "だして"
    },
    {
        "id": 13,
        "display": "出かける",
        "dictionary": "でかける",
        "romaji": "dekakeru",
        "meaning": "To go out",
        "masu": "でかけます",
        "particle": "に/へ",
        "sentence": "しゅうまつにかいものへ出かけます。",
        "isN5Kanji": true,
        "te": "でかけて"
    },
    {
        "id": 14,
        "display": "電話する",
        "dictionary": "でんわする",
        "romaji": "denwa suru",
        "meaning": "To telephone / call",
        "masu": "でんわします",
        "particle": "に",
        "sentence": "あとで母に電話します。",
        "isN5Kanji": true,
        "te": "でんわして"
    },
    {
        "id": 15,
        "display": "出る",
        "dictionary": "でる",
        "romaji": "deru",
        "meaning": "To leave / exit / appear",
        "masu": "でます",
        "particle": "を/から",
        "sentence": "しちじにうちを出ます。",
        "isN5Kanji": true,
        "te": "でて"
    },
    {
        "id": 16,
        "display": "吹く",
        "dictionary": "ふく",
        "romaji": "fuku",
        "meaning": "To blow",
        "masu": "ふきます",
        "particle": "が",
        "sentence": "つよいかぜが吹きます。",
        "isN5Kanji": true,
        "te": "ふいて"
    },
    {
        "id": 17,
        "display": "降る",
        "dictionary": "ふる",
        "romaji": "furu",
        "meaning": "To fall (rain / snow)",
        "masu": "ふります",
        "particle": "が",
        "sentence": "あした、あめが降ります。",
        "isN5Kanji": true,
        "te": "ふって"
    },
    {
        "id": 18,
        "display": "入る",
        "dictionary": "はいる",
        "romaji": "hairu",
        "meaning": "To enter",
        "masu": "はいります",
        "particle": "に",
        "sentence": "へやに入ります。",
        "isN5Kanji": true,
        "te": "はいって"
    },
    {
        "id": 19,
        "display": "始まる",
        "dictionary": "はじまる",
        "romaji": "hajimaru",
        "meaning": "To begin",
        "masu": "はじまります",
        "particle": "が",
        "sentence": "じゅぎょうが九時に始まります。",
        "isN5Kanji": true,
        "te": "はじまって"
    },
    {
        "id": 20,
        "display": "履く",
        "dictionary": "はく",
        "romaji": "haku",
        "meaning": "To wear (lower body / footwear)",
        "masu": "はきます",
        "particle": "を",
        "sentence": "くつを履きます。",
        "isN5Kanji": true,
        "te": "はいて"
    },
    {
        "id": 21,
        "display": "話す",
        "dictionary": "はなす",
        "romaji": "hanasu",
        "meaning": "To speak / talk",
        "masu": "はなします",
        "particle": "を/と",
        "sentence": "せんせいと日本語で話します。",
        "isN5Kanji": true,
        "te": "はなして"
    },
    {
        "id": 22,
        "display": "晴れる",
        "dictionary": "はれる",
        "romaji": "hareru",
        "meaning": "To clear up / become sunny",
        "masu": "はれます",
        "particle": "が",
        "sentence": "あしたは晴れます。",
        "isN5Kanji": true,
        "te": "はれて"
    },
    {
        "id": 23,
        "display": "貼る",
        "dictionary": "はる",
        "romaji": "haru",
        "meaning": "To stick / paste",
        "masu": "はります",
        "particle": "に/を",
        "sentence": "かべにポスターを貼ります。",
        "isN5Kanji": true,
        "te": "はって"
    },
    {
        "id": 24,
        "display": "走る",
        "dictionary": "はしる",
        "romaji": "hashiru",
        "meaning": "To run",
        "masu": "はしります",
        "particle": "を/で",
        "sentence": "こうえんを走ります。",
        "isN5Kanji": true,
        "te": "はしって"
    },
    {
        "id": 25,
        "display": "働く",
        "dictionary": "はたらく",
        "romaji": "hataraku",
        "meaning": "To work",
        "masu": "はたらきます",
        "particle": "で",
        "sentence": "ぎんこうで働きます。",
        "isN5Kanji": true,
        "te": "はたらいて"
    },
    {
        "id": 26,
        "display": "引く",
        "dictionary": "ひく",
        "romaji": "hiku",
        "meaning": "To pull / consult",
        "masu": "ひきます",
        "particle": "を",
        "sentence": "ドアを引きます。",
        "isN5Kanji": true,
        "te": "ひいて"
    },
    {
        "id": 27,
        "display": "弾く",
        "dictionary": "ひく",
        "romaji": "hiku",
        "meaning": "To play a stringed or keyboard instrument",
        "masu": "ひきます",
        "particle": "を",
        "sentence": "ピアノを弾きます。",
        "isN5Kanji": true,
        "te": "ひいて"
    },
    {
        "id": 28,
        "display": "行く",
        "dictionary": "いく",
        "romaji": "iku",
        "meaning": "To go",
        "masu": "いきます",
        "particle": "に/へ",
        "sentence": "がっこうに行きます。",
        "isN5Kanji": true,
        "te": "いって"
    },
    {
        "id": 29,
        "display": "入れる",
        "dictionary": "いれる",
        "romaji": "ireru",
        "meaning": "To put in / let in",
        "masu": "いれます",
        "particle": "に/を",
        "sentence": "コーヒーにさとうを入れます。",
        "isN5Kanji": true,
        "te": "いれて"
    },
    {
        "id": 30,
        "display": "要る",
        "dictionary": "いる",
        "romaji": "iru",
        "meaning": "To need",
        "masu": "いります",
        "particle": "が",
        "sentence": "りょこうにはおかねが要ります。",
        "isN5Kanji": true,
        "te": "いって"
    },
    {
        "id": 31,
        "display": "居る",
        "dictionary": "いる",
        "romaji": "iru",
        "meaning": "To exist / be (animate)",
        "masu": "います",
        "particle": "が/に",
        "sentence": "へやにねこが居ます。",
        "isN5Kanji": true,
        "te": "いって"
    },
    {
        "id": 32,
        "display": "言う",
        "dictionary": "いう",
        "romaji": "iu",
        "meaning": "To say / call",
        "masu": "いいます",
        "particle": "と",
        "sentence": "「ありがとう」と言います。",
        "isN5Kanji": true,
        "te": "いって"
    },
    {
        "id": 33,
        "display": "授業を受ける",
        "dictionary": "じゅぎょうをうける",
        "romaji": "jugyou o ukeru",
        "meaning": "To attend / take a class",
        "masu": "じゅぎょうをうけます",
        "particle": "を",
        "sentence": "ごぜん、日本語の授業を受けます。",
        "isN5Kanji": true,
        "te": "じゅぎょうをうけて"
    },
    {
        "id": 34,
        "display": "帰る",
        "dictionary": "かえる",
        "romaji": "kaeru",
        "meaning": "To return / go home",
        "masu": "かえります",
        "particle": "に/へ",
        "sentence": "六時にうちへ帰ります。",
        "isN5Kanji": true,
        "te": "かえって"
    },
    {
        "id": 35,
        "display": "返す",
        "dictionary": "かえす",
        "romaji": "kaesu",
        "meaning": "To return something",
        "masu": "かえします",
        "particle": "に/を",
        "sentence": "ともだちにほんを返します。",
        "isN5Kanji": true,
        "te": "かえして"
    },
    {
        "id": 36,
        "display": "掛かる",
        "dictionary": "かかる",
        "romaji": "kakaru",
        "meaning": "To take (time / money)",
        "masu": "かかります",
        "particle": "が",
        "sentence": "えきまで三十分掛かります。",
        "isN5Kanji": true,
        "te": "かかって"
    },
    {
        "id": 37,
        "display": "掛ける",
        "dictionary": "かける",
        "romaji": "kakeru",
        "meaning": "To hang / make a call",
        "masu": "かけます",
        "particle": "に/を",
        "sentence": "かべにえを掛けます。",
        "isN5Kanji": true,
        "te": "かけて"
    },
    {
        "id": 38,
        "display": "書く",
        "dictionary": "かく",
        "romaji": "kaku",
        "meaning": "To write / draw",
        "masu": "かきます",
        "particle": "を",
        "sentence": "ノートになまえを書きます。",
        "isN5Kanji": true,
        "te": "かいて"
    },
    {
        "id": 39,
        "display": "借りる",
        "dictionary": "かりる",
        "romaji": "kariru",
        "meaning": "To borrow / rent",
        "masu": "かります",
        "particle": "に/から/を",
        "sentence": "としょかんでほんを借ります。",
        "isN5Kanji": true,
        "te": "かりて"
    },
    {
        "id": 40,
        "display": "貸す",
        "dictionary": "かす",
        "romaji": "kasu",
        "meaning": "To lend",
        "masu": "かします",
        "particle": "に/を",
        "sentence": "ともだちにペンを貸します。",
        "isN5Kanji": true,
        "te": "かして"
    },
    {
        "id": 41,
        "display": "買う",
        "dictionary": "かう",
        "romaji": "kau",
        "meaning": "To buy",
        "masu": "かいます",
        "particle": "を",
        "sentence": "スーパーでやさいを買います。",
        "isN5Kanji": true,
        "te": "かって"
    },
    {
        "id": 42,
        "display": "結婚する",
        "dictionary": "けっこんする",
        "romaji": "kekkon suru",
        "meaning": "To marry / get married",
        "masu": "けっこんします",
        "particle": "と",
        "sentence": "らいねん、かれと結婚します。",
        "isN5Kanji": true,
        "te": "けっこんして"
    },
    {
        "id": 43,
        "display": "消す",
        "dictionary": "けす",
        "romaji": "kesu",
        "meaning": "To erase / turn off",
        "masu": "けします",
        "particle": "を",
        "sentence": "でんきを消します。",
        "isN5Kanji": true,
        "te": "けして"
    },
    {
        "id": 44,
        "display": "消える",
        "dictionary": "きえる",
        "romaji": "kieru",
        "meaning": "To disappear / go out",
        "masu": "きえます",
        "particle": "が",
        "sentence": "でんきが消えました。",
        "isN5Kanji": true,
        "te": "きえて"
    },
    {
        "id": 45,
        "display": "聞く",
        "dictionary": "きく",
        "romaji": "kiku",
        "meaning": "To hear / listen / ask",
        "masu": "ききます",
        "particle": "を/に",
        "sentence": "おんがくを聞きます。",
        "isN5Kanji": true,
        "te": "きいて"
    },
    {
        "id": 46,
        "display": "切る",
        "dictionary": "きる",
        "romaji": "kiru",
        "meaning": "To cut",
        "masu": "きります",
        "particle": "を",
        "sentence": "はさみでかみを切ります。",
        "isN5Kanji": true,
        "te": "きて"
    },
    {
        "id": 47,
        "display": "着る",
        "dictionary": "きる",
        "romaji": "kiru",
        "meaning": "To wear (upper body)",
        "masu": "きます",
        "particle": "を",
        "sentence": "コートを着ます。",
        "isN5Kanji": true,
        "te": "きて"
    },
    {
        "id": 48,
        "display": "困る",
        "dictionary": "こまる",
        "romaji": "komaru",
        "meaning": "To be troubled",
        "masu": "こまります",
        "particle": "に/で",
        "sentence": "おかねがなくて困ります。",
        "isN5Kanji": true,
        "te": "こまって"
    },
    {
        "id": 49,
        "display": "コピーする",
        "dictionary": "コピーする",
        "romaji": "kopii suru",
        "meaning": "To copy / photocopy",
        "masu": "コピーします",
        "particle": "を",
        "sentence": "このページをコピーします。",
        "isN5Kanji": true,
        "te": "コピーして"
    },
    {
        "id": 50,
        "display": "答える",
        "dictionary": "こたえる",
        "romaji": "kotaeru",
        "meaning": "To answer",
        "masu": "こたえます",
        "particle": "に",
        "sentence": "しつもんに答えます。",
        "isN5Kanji": true,
        "te": "こたえて"
    },
    {
        "id": 51,
        "display": "曇る",
        "dictionary": "くもる",
        "romaji": "kumoru",
        "meaning": "To become cloudy",
        "masu": "くもります",
        "particle": "が",
        "sentence": "ごごから曇ります。",
        "isN5Kanji": true,
        "te": "くもって"
    },
    {
        "id": 52,
        "display": "来る",
        "dictionary": "くる",
        "romaji": "kuru",
        "meaning": "To come",
        "masu": "きます",
        "particle": "に/へ",
        "sentence": "ともだちがうちに来ます。",
        "isN5Kanji": true,
        "te": "きて"
    },
    {
        "id": 53,
        "display": "曲がる",
        "dictionary": "まがる",
        "romaji": "magaru",
        "meaning": "To turn / bend",
        "masu": "まがります",
        "particle": "を/に",
        "sentence": "つぎのかどを右に曲がります。",
        "isN5Kanji": true,
        "te": "まがって"
    },
    {
        "id": 54,
        "display": "待つ",
        "dictionary": "まつ",
        "romaji": "matsu",
        "meaning": "To wait",
        "masu": "まちます",
        "particle": "を",
        "sentence": "えきでともだちを待ちます。",
        "isN5Kanji": true,
        "te": "まって"
    },
    {
        "id": 55,
        "display": "磨く",
        "dictionary": "みがく",
        "romaji": "migaku",
        "meaning": "To polish / brush",
        "masu": "みがきます",
        "particle": "を",
        "sentence": "まいあさ、はを磨きます。",
        "isN5Kanji": true,
        "te": "みがいて"
    },
    {
        "id": 56,
        "display": "見る",
        "dictionary": "みる",
        "romaji": "miru",
        "meaning": "To see / watch",
        "masu": "みます",
        "particle": "を",
        "sentence": "よる、テレビを見ます。",
        "isN5Kanji": true,
        "te": "みて"
    },
    {
        "id": 57,
        "display": "見せる",
        "dictionary": "みせる",
        "romaji": "miseru",
        "meaning": "To show",
        "masu": "みせます",
        "particle": "に/を",
        "sentence": "せんせいにしゃしんを見せます。",
        "isN5Kanji": true,
        "te": "みせて"
    },
    {
        "id": 58,
        "display": "持つ",
        "dictionary": "もつ",
        "romaji": "motsu",
        "meaning": "To hold / carry / have",
        "masu": "もちます",
        "particle": "を",
        "sentence": "かばんを持ちます。",
        "isN5Kanji": true,
        "te": "もって"
    },
    {
        "id": 59,
        "display": "鳴く",
        "dictionary": "なく",
        "romaji": "naku",
        "meaning": "To make an animal sound",
        "masu": "なきます",
        "particle": "が",
        "sentence": "あさ、とりが鳴きます。",
        "isN5Kanji": true,
        "te": "ないて"
    },
    {
        "id": 60,
        "display": "無くす",
        "dictionary": "なくす",
        "romaji": "nakusu",
        "meaning": "To lose something",
        "masu": "なくします",
        "particle": "を",
        "sentence": "かぎを無くしました。",
        "isN5Kanji": true,
        "te": "なくして"
    },
    {
        "id": 61,
        "display": "並べる",
        "dictionary": "ならべる",
        "romaji": "naraberu",
        "meaning": "To arrange / line things up",
        "masu": "ならべます",
        "particle": "を",
        "sentence": "つくえにほんを並べます。",
        "isN5Kanji": true,
        "te": "ならべて"
    },
    {
        "id": 62,
        "display": "並ぶ",
        "dictionary": "ならぶ",
        "romaji": "narabu",
        "meaning": "To line up",
        "masu": "ならびます",
        "particle": "に",
        "sentence": "みせのまえにひとが並びます。",
        "isN5Kanji": true,
        "te": "ならんで"
    },
    {
        "id": 63,
        "display": "習う",
        "dictionary": "ならう",
        "romaji": "narau",
        "meaning": "To learn from a teacher",
        "masu": "ならいます",
        "particle": "を/に",
        "sentence": "せんせいに日本語を習います。",
        "isN5Kanji": true,
        "te": "ならって"
    },
    {
        "id": 64,
        "display": "寝る",
        "dictionary": "ねる",
        "romaji": "neru",
        "meaning": "To sleep / go to bed",
        "masu": "ねます",
        "particle": "に",
        "sentence": "十一時に寝ます。",
        "isN5Kanji": true,
        "te": "ねて"
    },
    {
        "id": 65,
        "display": "登る",
        "dictionary": "のぼる",
        "romaji": "noboru",
        "meaning": "To climb",
        "masu": "のぼります",
        "particle": "に/を",
        "sentence": "やまに登ります。",
        "isN5Kanji": true,
        "te": "のぼって"
    },
    {
        "id": 66,
        "display": "飲む",
        "dictionary": "のむ",
        "romaji": "nomu",
        "meaning": "To drink",
        "masu": "のみます",
        "particle": "を",
        "sentence": "みずを飲みます。",
        "isN5Kanji": true,
        "te": "のんで"
    },
    {
        "id": 67,
        "display": "乗る",
        "dictionary": "のる",
        "romaji": "noru",
        "meaning": "To ride / board",
        "masu": "のります",
        "particle": "に",
        "sentence": "でんしゃに乗ります。",
        "isN5Kanji": true,
        "te": "のって"
    },
    {
        "id": 68,
        "display": "脱ぐ",
        "dictionary": "ぬぐ",
        "romaji": "nugu",
        "meaning": "To take off clothes",
        "masu": "ぬぎます",
        "particle": "を",
        "sentence": "げんかんでくつを脱ぎます。",
        "isN5Kanji": true,
        "te": "ぬいで"
    },
    {
        "id": 69,
        "display": "覚える",
        "dictionary": "おぼえる",
        "romaji": "oboeru",
        "meaning": "To remember / memorize",
        "masu": "おぼえます",
        "particle": "を",
        "sentence": "あたらしいたんごを覚えます。",
        "isN5Kanji": true,
        "te": "おぼえて"
    },
    {
        "id": 70,
        "display": "起きる",
        "dictionary": "おきる",
        "romaji": "okiru",
        "meaning": "To wake up / get up",
        "masu": "おきます",
        "particle": "に",
        "sentence": "まいあさ六時に起きます。",
        "isN5Kanji": true,
        "te": "おきて"
    },
    {
        "id": 71,
        "display": "置く",
        "dictionary": "おく",
        "romaji": "oku",
        "meaning": "To put / place",
        "masu": "おきます",
        "particle": "に/を",
        "sentence": "つくえのうえにほんを置きます。",
        "isN5Kanji": true,
        "te": "おいて"
    },
    {
        "id": 72,
        "display": "降りる",
        "dictionary": "おりる",
        "romaji": "oriru",
        "meaning": "To get off / descend",
        "masu": "おります",
        "particle": "を/から",
        "sentence": "つぎのえきででんしゃを降ります。",
        "isN5Kanji": true,
        "te": "おりて"
    },
    {
        "id": 73,
        "display": "教える",
        "dictionary": "おしえる",
        "romaji": "oshieru",
        "meaning": "To teach / tell",
        "masu": "おしえます",
        "particle": "に/を",
        "sentence": "せんせいが日本語を教えます。",
        "isN5Kanji": true,
        "te": "おしえて"
    },
    {
        "id": 74,
        "display": "押す",
        "dictionary": "おす",
        "romaji": "osu",
        "meaning": "To push / press",
        "masu": "おします",
        "particle": "を",
        "sentence": "このボタンを押してください。",
        "isN5Kanji": true,
        "te": "おして"
    },
    {
        "id": 75,
        "display": "終わる",
        "dictionary": "おわる",
        "romaji": "owaru",
        "meaning": "To finish / end",
        "masu": "おわります",
        "particle": "が",
        "sentence": "しごとは五時に終わります。",
        "isN5Kanji": true,
        "te": "おわって"
    },
    {
        "id": 76,
        "display": "泳ぐ",
        "dictionary": "およぐ",
        "romaji": "oyogu",
        "meaning": "To swim",
        "masu": "およぎます",
        "particle": "で/を",
        "sentence": "プールで泳ぎます。",
        "isN5Kanji": true,
        "te": "およいで"
    },
    {
        "id": 77,
        "display": "練習する",
        "dictionary": "れんしゅうする",
        "romaji": "renshuu suru",
        "meaning": "To practise",
        "masu": "れんしゅうします",
        "particle": "を",
        "sentence": "まいにち、かいわを練習します。",
        "isN5Kanji": true,
        "te": "れんしゅうして"
    },
    {
        "id": 78,
        "display": "旅行する",
        "dictionary": "りょこうする",
        "romaji": "ryokou suru",
        "meaning": "To travel",
        "masu": "りょこうします",
        "particle": "に/へ",
        "sentence": "らいげつ、日本を旅行します。",
        "isN5Kanji": true,
        "te": "りょこうして"
    },
    {
        "id": 79,
        "display": "料理する",
        "dictionary": "りょうりする",
        "romaji": "ryouri suru",
        "meaning": "To cook",
        "masu": "りょうりします",
        "particle": "を",
        "sentence": "ばんごはんを料理します。",
        "isN5Kanji": true,
        "te": "りょうりして"
    },
    {
        "id": 80,
        "display": "咲く",
        "dictionary": "さく",
        "romaji": "saku",
        "meaning": "To bloom",
        "masu": "さきます",
        "particle": "が",
        "sentence": "はるにはなが咲きます。",
        "isN5Kanji": true,
        "te": "さいて"
    },
    {
        "id": 81,
        "display": "散歩する",
        "dictionary": "さんぽする",
        "romaji": "sanpo suru",
        "meaning": "To take a walk",
        "masu": "さんぽします",
        "particle": "を/で",
        "sentence": "まいあさ、こうえんを散歩します。",
        "isN5Kanji": true,
        "te": "さんぽして"
    },
    {
        "id": 82,
        "display": "差す",
        "dictionary": "さす",
        "romaji": "sasu",
        "meaning": "To hold up (an umbrella)",
        "masu": "さします",
        "particle": "を",
        "sentence": "あめのひにかさを差します。",
        "isN5Kanji": true,
        "te": "さして"
    },
    {
        "id": 83,
        "display": "洗濯する",
        "dictionary": "せんたくする",
        "romaji": "sentaku suru",
        "meaning": "To do laundry",
        "masu": "せんたくします",
        "particle": "を",
        "sentence": "日曜日にふくを洗濯します。",
        "isN5Kanji": true,
        "te": "せんたくして"
    },
    {
        "id": 84,
        "display": "仕事をする",
        "dictionary": "しごとをする",
        "romaji": "shigoto o suru",
        "meaning": "To work / do a job",
        "masu": "しごとをします",
        "particle": "を/で",
        "sentence": "かいしゃで仕事をします。",
        "isN5Kanji": true,
        "te": "しごとをして"
    },
    {
        "id": 85,
        "display": "閉まる",
        "dictionary": "しまる",
        "romaji": "shimaru",
        "meaning": "To close (intransitive)",
        "masu": "しまります",
        "particle": "が",
        "sentence": "みせは八時に閉まります。",
        "isN5Kanji": true,
        "te": "しまって"
    },
    {
        "id": 86,
        "display": "閉める",
        "dictionary": "しめる",
        "romaji": "shimeru",
        "meaning": "To close (something)",
        "masu": "しめます",
        "particle": "を",
        "sentence": "ドアを閉めます。",
        "isN5Kanji": true,
        "te": "しめて"
    },
    {
        "id": 87,
        "display": "締める",
        "dictionary": "しめる",
        "romaji": "shimeru",
        "meaning": "To tie / fasten / tighten",
        "masu": "しめます",
        "particle": "を",
        "sentence": "ネクタイを締めます。",
        "isN5Kanji": true,
        "te": "しめて"
    },
    {
        "id": 88,
        "display": "死ぬ",
        "dictionary": "しぬ",
        "romaji": "shinu",
        "meaning": "To die",
        "masu": "しにます",
        "particle": "が",
        "sentence": "そのむしは冬に死にます。",
        "isN5Kanji": true,
        "te": "しんで"
    },
    {
        "id": 89,
        "display": "知る",
        "dictionary": "しる",
        "romaji": "shiru",
        "meaning": "To know / learn of",
        "masu": "しります",
        "particle": "を",
        "sentence": "そのニュースを知っています。",
        "isN5Kanji": true,
        "te": "しって"
    },
    {
        "id": 90,
        "display": "質問する",
        "dictionary": "しつもんする",
        "romaji": "shitsumon suru",
        "meaning": "To ask a question",
        "masu": "しつもんします",
        "particle": "に",
        "sentence": "せんせいに質問します。",
        "isN5Kanji": true,
        "te": "しつもんして"
    },
    {
        "id": 91,
        "display": "掃除する",
        "dictionary": "そうじする",
        "romaji": "souji suru",
        "meaning": "To clean",
        "masu": "そうじします",
        "particle": "を",
        "sentence": "まいあさ、へやを掃除します。",
        "isN5Kanji": true,
        "te": "そうじして"
    },
    {
        "id": 92,
        "display": "住む",
        "dictionary": "すむ",
        "romaji": "sumu",
        "meaning": "To live / reside",
        "masu": "すみます",
        "particle": "に",
        "sentence": "リヤドに住んでいます。",
        "isN5Kanji": true,
        "te": "すんで"
    },
    {
        "id": 93,
        "display": "吸う",
        "dictionary": "すう",
        "romaji": "suu",
        "meaning": "To smoke / inhale / suck",
        "masu": "すいます",
        "particle": "を",
        "sentence": "ここでたばこを吸わないでください。",
        "isN5Kanji": true,
        "te": "すって"
    },
    {
        "id": 94,
        "display": "座る",
        "dictionary": "すわる",
        "romaji": "suwaru",
        "meaning": "To sit",
        "masu": "すわります",
        "particle": "に",
        "sentence": "いすに座ります。",
        "isN5Kanji": true,
        "te": "すわって"
    },
    {
        "id": 95,
        "display": "食べる",
        "dictionary": "たべる",
        "romaji": "taberu",
        "meaning": "To eat",
        "masu": "たべます",
        "particle": "を",
        "sentence": "あさごはんを食べます。",
        "isN5Kanji": true,
        "te": "たべて"
    },
    {
        "id": 96,
        "display": "頼む",
        "dictionary": "たのむ",
        "romaji": "tanomu",
        "meaning": "To ask / request / order",
        "masu": "たのみます",
        "particle": "に/を",
        "sentence": "レストランでコーヒーを頼みます。",
        "isN5Kanji": true,
        "te": "たのんで"
    },
    {
        "id": 97,
        "display": "立つ",
        "dictionary": "たつ",
        "romaji": "tatsu",
        "meaning": "To stand",
        "masu": "たちます",
        "particle": "に/から",
        "sentence": "いすから立ちます。",
        "isN5Kanji": true,
        "te": "たって"
    },
    {
        "id": 98,
        "display": "テストを受ける",
        "dictionary": "テストをうける",
        "romaji": "tesuto o ukeru",
        "meaning": "To take a test",
        "masu": "テストをうけます",
        "particle": "を",
        "sentence": "きょう、日本語のテストを受けます。",
        "isN5Kanji": true,
        "te": "テストをうけて"
    },
    {
        "id": 99,
        "display": "飛ぶ",
        "dictionary": "とぶ",
        "romaji": "tobu",
        "meaning": "To fly / jump",
        "masu": "とびます",
        "particle": "が/を",
        "sentence": "そらをとりが飛びます。",
        "isN5Kanji": true,
        "te": "とんで"
    },
    {
        "id": 100,
        "display": "止まる",
        "dictionary": "とまる",
        "romaji": "tomaru",
        "meaning": "To stop",
        "masu": "とまります",
        "particle": "が/に",
        "sentence": "バスがえきに止まります。",
        "isN5Kanji": true,
        "te": "とまって"
    }
];
</script>

<!-- Header and Stats HUD Banner -->
<header class="bg-indigo-700 text-white shadow-md">
    <div class="max-w-6xl mx-auto px-4 py-4 flex flex-col md:flex-row items-center justify-between gap-4">
        <div class="flex items-center gap-3">
            <span class="material-icons text-3xl">school</span>
            <div>
                <h1 class="text-xl font-bold tracking-tight">Japanese JLPT N5 Verbs</h1>
                <p class="text-xs text-indigo-200">100 core verbs and verb expressions</p>
            </div>
        </div>
        <div class="flex flex-wrap gap-2">
            <button id="btn-quiz-mode" onclick="setAppView('quiz')" class="px-4 py-2 rounded-lg bg-indigo-800 text-white border border-indigo-600 font-semibold text-sm hover:bg-indigo-900 transition flex items-center gap-2">
                <span class="material-icons text-sm">play_circle</span> Quiz Mode
            </button>
            <button id="btn-table-mode" onclick="setAppView('table')" class="px-4 py-2 rounded-lg bg-indigo-600 text-white font-semibold text-sm hover:bg-indigo-500 transition flex items-center gap-2">
                <span class="material-icons text-sm">menu_book</span> Reference (100 Verbs)
            </button>
            <button onclick="openPrintFriendly()" class="px-4 py-2 rounded-lg bg-emerald-600 hover:bg-emerald-500 text-white font-semibold text-sm transition flex items-center gap-2">
                <span class="material-icons text-sm">open_in_new</span> Open Printer Friendly
            </button>
        </div>
    </div>
</header>

<main class="flex-grow max-w-6xl w-full mx-auto px-4 py-6 flex flex-col gap-6">

    <!-- Active view tracker banner -->
    <div id="stats-hud" class="grid grid-cols-2 sm:grid-cols-4 gap-4 bg-white p-4 rounded-xl shadow-sm border border-slate-100">
        <div class="flex flex-col">
            <span class="text-xs text-slate-400 uppercase tracking-wider font-semibold">Active Mode</span>
            <span id="hud-mode" class="text-base font-bold text-indigo-700">Quiz</span>
        </div>
        <div class="flex flex-col border-l border-slate-100 pl-4">
            <span class="text-xs text-slate-400 uppercase tracking-wider font-semibold">Current Score</span>
            <span id="hud-score" class="text-base font-bold text-slate-700">0 / 0</span>
        </div>
        <div class="flex flex-col border-l border-slate-100 pl-4">
            <span class="text-xs text-slate-400 uppercase tracking-wider font-semibold">Dataset</span>
            <span class="text-base font-bold text-emerald-600 flex items-center gap-1">
                <span class="material-icons text-sm">check_circle</span> Curated
            </span>
        </div>
        <div class="flex flex-col border-l border-slate-100 pl-4">
            <span class="text-xs text-slate-400 uppercase tracking-wider font-semibold">Total Vocabulary</span>
            <span class="text-base font-bold text-slate-700"><span id="hud-total-verbs">100 Verbs Loaded</span></span>
        </div>
    </div>

    <!-- VIEW 1: QUIZ CONTAINER -->
    <section id="view-quiz" class="flex flex-col gap-6">
        <!-- Main Quiz Card -->
        <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 p-6 md:p-8 flex flex-col gap-6 max-w-2xl mx-auto w-full">
            
            <div class="flex items-center justify-between border-b border-slate-100 pb-4">
                <div>
                    <span id="quiz-question-index" class="text-xs text-indigo-600 font-semibold uppercase tracking-widest">Question 1 of 10</span>
                    <h2 class="text-lg font-bold text-slate-800">Complete the Sentence or Conversion</h2>
                </div>
                <button onclick="resetQuiz()" class="text-slate-400 hover:text-slate-600 p-1 rounded-full hover:bg-slate-50 transition" title="Restart Quiz">
                    <span class="material-icons">replay</span>
                </button>
            </div>

            <!-- Quiz Display Window -->
            <div class="flex flex-col gap-6 min-h-[160px] justify-center">
                <!-- Question Prompt -->
                <div id="quiz-prompt-container" class="flex flex-col gap-2">
                    <span id="quiz-badge-type" class="w-fit px-2 py-1 bg-indigo-50 text-indigo-700 rounded text-[10px] font-bold tracking-wider uppercase">Masu-Form Challenge</span>
                    <p id="quiz-prompt" class="text-xl md:text-2xl font-semibold text-slate-800 japanese-text leading-relaxed">Loading prompt...</p>
                    <p id="quiz-prompt-helper" class="text-sm text-slate-500">Choose the correct answer.</p>
                </div>

                <!-- Live Notification / Toast area -->
                <div id="feedback-alert" class="hidden flex items-center gap-3 p-4 rounded-xl border font-medium transition duration-200">
                    <span id="feedback-icon" class="material-icons"></span>
                    <span id="feedback-text"></span>
                </div>

                <!-- Answer options (Multiple Choice) -->
                <div id="quiz-options-container" class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                    <!-- Dynamic -->
                </div>

                <!-- Typed Entry Challenge -->
                <div id="quiz-text-entry-container" class="hidden flex flex-col gap-3">
                    <label for="typed-answer" class="text-sm font-semibold text-slate-600">Type in your Hiragana response:</label>
                    <div class="flex gap-2">
                        <input id="typed-answer" type="text" placeholder="Type inside here..." class="flex-grow bg-slate-50 border border-slate-200 rounded-xl px-4 py-3 text-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 font-medium">
                        <button id="btn-typed-submit" onclick="submitTypedResponse()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-6 py-3 rounded-xl font-semibold transition flex items-center gap-2">
                            Submit
                        </button>
                    </div>
                </div>
            </div>

            <!-- Quiz Navigation Controls -->
            <div class="flex items-center justify-between gap-4 border-t border-slate-100 pt-4 mt-2">
                <button id="btn-prev-question" onclick="goToPrevQuestion()" class="px-4 py-2.5 rounded-xl border border-slate-200 text-slate-600 hover:bg-slate-50 transition flex items-center gap-2 disabled:opacity-40 disabled:hover:bg-transparent disabled:pointer-events-none text-sm font-semibold">
                    <span class="material-icons text-sm">arrow_back</span> Previous
                </button>
                <button id="btn-next-question" onclick="goToNextQuestion()" class="px-4 py-2.5 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white transition flex items-center gap-2 disabled:opacity-40 disabled:pointer-events-none text-sm font-semibold">
                    Next <span class="material-icons text-sm">arrow_forward</span>
                </button>
            </div>

            <!-- Quiz Footer Progression -->
            <div class="border-t border-slate-100 pt-4 flex items-center justify-between">
                <div class="flex-grow bg-slate-100 rounded-full h-2 overflow-hidden mr-4">
                    <div id="quiz-progress-bar" class="bg-indigo-600 h-full transition-all duration-300" style="width: 10%;"></div>
                </div>
                <span id="quiz-progress-text" class="text-xs font-medium text-slate-400 whitespace-nowrap">10% Complete</span>
            </div>

        </div>
    </section>

    <!-- VIEW 2: REFERENCE TABLE -->
    <section id="view-table" class="hidden flex flex-col gap-4">
        <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 p-6 flex flex-col gap-6">
            
            <!-- Table Action Buttons & Filters -->
            <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                <div>
                    <h2 class="text-xl font-bold text-slate-800">JLPT N5 Verbs Database</h2>
                    <p class="text-sm text-slate-500">N5-approved kanji with furigana, hiragana, meanings, conjugations, particles, and examples</p>
                </div>
                
                <!-- Quick Search Input -->
                <div class="relative w-full md:w-80">
                    <span class="material-icons absolute left-3 top-1/2 -translate-y-1/2 text-slate-400">search</span>
                    <input type="text" id="table-search" oninput="handleTableSearch(this.value)" placeholder="Search meaning, hiragana..." class="w-full pl-10 pr-4 py-2 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                </div>
            </div>

            <!-- Action buttons: CSV download, Copy CSV -->
            <div class="flex flex-wrap gap-2 border-b border-slate-100 pb-4">
                <button onclick="downloadCSV()" class="px-4 py-2 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white text-xs font-semibold flex items-center gap-1.5 shadow-sm transition">
                    <span class="material-icons text-sm">download</span> Download CSV File
                </button>
                <button onclick="copyCSVToClipboard()" class="px-4 py-2 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-semibold flex items-center gap-1.5 transition">
                    <span class="material-icons text-sm">content_copy</span> Copy CSV Dataset
                </button>
                <button onclick="printTableData()" class="px-4 py-2 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-semibold flex items-center gap-1.5 transition">
                    <span class="material-icons text-sm">print</span> Print Table
                </button>
            </div>

            <!-- Table Container -->
            <div class="overflow-x-auto border border-slate-100 rounded-xl">
                <table class="min-w-full divide-y divide-slate-200 text-sm text-left">
                    <thead class="bg-slate-50 text-slate-600 uppercase text-xs font-semibold tracking-wider">
                        <tr>
                            <th scope="col" class="px-6 py-4">ID</th>
                            <th scope="col" class="px-6 py-4">Verb</th>
                            <th scope="col" class="px-6 py-4">Hiragana</th>
                            <th scope="col" class="px-6 py-4">Romaji</th>
                            <th scope="col" class="px-6 py-4">Meaning</th>
                            <th scope="col" class="px-6 py-4">Polite (Masu)</th>
                            <th scope="col" class="px-6 py-4">て-form</th>
                            <th scope="col" class="px-6 py-4">Particle</th>
                            <th scope="col" class="px-6 py-4">Example Sentence</th>
                        </tr>
                    </thead>
                    <tbody id="verbs-table-body" class="divide-y divide-slate-100 bg-white">
                        <!-- Rendered Dynamically -->
                    </tbody>
                </table>
            </div>

            <div id="table-empty-state" class="hidden text-center py-12">
                <span class="material-icons text-slate-300 text-4xl mb-2">find_in_page</span>
                <p class="text-slate-500 font-medium">No verbs found matching your search term.</p>
            </div>

        </div>
    </section>

</main>

<!-- Beautiful Custom Toast Notification Box -->
<div id="toast-banner" class="fixed bottom-5 right-5 z-50 bg-slate-900 text-white px-5 py-3 rounded-xl shadow-xl flex items-center gap-3 transition-all duration-300 transform translate-y-20 opacity-0 pointer-events-none">
    <span class="material-icons text-emerald-400" id="toast-icon">check_circle</span>
    <span class="text-sm font-medium" id="toast-text">Message placeholder</span>
</div>

<!-- Print-Friendly Full Database Overlay Modal -->
<div id="print-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
    <div class="bg-white rounded-2xl shadow-2xl max-w-5xl w-full h-[90vh] flex flex-col overflow-hidden animate-in fade-in zoom-in duration-200">
        <!-- Modal Header -->
        <div class="p-6 border-b border-slate-100 flex items-center justify-between bg-slate-50">
            <div>
                <h3 class="text-lg font-bold text-slate-800">Japanese N5 Verbs — Offline Study & Print View</h3>
                <p class="text-xs text-slate-500">A curated practice list with natural beginner-level examples.</p>
            </div>
            <div class="flex items-center gap-2">
                <button onclick="triggerPrintWindow()" class="px-4 py-2 bg-emerald-600 hover:bg-emerald-500 text-white rounded-xl text-xs font-semibold flex items-center gap-1.5 transition">
                    <span class="material-icons text-sm">print</span> Print Now / Save PDF
                </button>
                <button onclick="closePrintFriendly()" class="p-2 bg-slate-200 hover:bg-slate-300 text-slate-700 rounded-full transition">
                    <span class="material-icons">close</span>
                </button>
            </div>
        </div>
        
        <!-- Modal Table Body / Print Zone -->
        <div id="print-container-frame" class="flex-grow overflow-y-auto p-8">
            <div id="print-view-document" class="text-slate-900">
                <div class="text-center pb-6 border-b-2 border-slate-900/10 mb-6">
                    <h1 class="text-2xl font-extrabold tracking-tight">100 Core JLPT N5 Japanese Verbs and Expressions</h1>
                    <p class="text-sm text-slate-600">Kanji, readings, polite forms, particles, and example sentences.</p>
                </div>
                <table class="min-w-full divide-y divide-slate-400 border border-slate-300 text-xs">
                    <thead>
                        <tr class="bg-slate-100 text-slate-900 font-bold">
                            <th class="px-3 py-2 border border-slate-300">#</th>
                            <th class="px-3 py-2 border border-slate-300">Verb</th>
                            <th class="px-3 py-2 border border-slate-300">Hiragana</th>
                            <th class="px-3 py-2 border border-slate-300">Romaji</th>
                            <th class="px-3 py-2 border border-slate-300">Polite Form (~masu)</th>
                            <th class="px-3 py-2 border border-slate-300">て-form</th>
                            <th class="px-3 py-2 border border-slate-300">Particle</th>
                            <th class="px-3 py-2 border border-slate-300">English Meaning</th>
                            <th class="px-3 py-2 border border-slate-300">Sample N5 Strict Sentence</th>
                        </tr>
                    </thead>
                    <tbody id="print-table-body" class="divide-y divide-slate-200">
                        <!-- Filled dynamically -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</div>

<script>
// State management variables
const QUIZ_LENGTH = 10;

let state = {
    view: 'quiz', // 'quiz' or 'table'
    currentQuestionIndex: 0,
    score: 0,
    quizFinished: false,
    typedResponse: "",
    feedback: null,
    questions: [],
    // Preserves individual answers and correct logs across previous navigation
    userAnswers: Array(QUIZ_LENGTH).fill(null),
    isCorrect: Array(QUIZ_LENGTH).fill(null),
    advanceTimeout: null
};


// N5 kanji list supplied by the learner.
// Kanji outside this list are displayed in hiragana.
const N5_KANJI = new Set([
    "日","一","国","人","年","大","十","二","本","中","長","出","三","時","行","見","月","分","後","前",
    "生","五","間","上","東","四","今","金","九","入","学","高","円","子","外","八","六","下","来","気",
    "小","七","山","話","女","北","午","百","書","先","名","川","千","水","半","男","西","電","校","語",
    "土","木","聞","食","車","何","南","万","毎","白","天","母","火","右","読","友","左","休","父","雨"
]);

function containsOnlyN5Kanji(text) {
    const kanji = [...text].filter(ch => /[\u3400-\u9FFF]/.test(ch));
    return kanji.length > 0 && kanji.every(ch => N5_KANJI.has(ch));
}

function getStudyText(verb) {
    return containsOnlyN5Kanji(verb.display) ? verb.display : verb.dictionary;
}

function getStudyDisplay(verb) {
    if (!containsOnlyN5Kanji(verb.display)) {
        return `<span class="japanese-text">${verb.dictionary}</span>`;
    }
    return `<ruby class="japanese-text">${verb.display}<rt>${verb.dictionary}</rt></ruby>`;
}

function getMeaningDistractors(verb, count = 3) {
    return [...N5_VERBS]
        .filter(v => v.id !== verb.id && v.meaning !== verb.meaning)
        .sort(() => Math.random() - 0.5)
        .slice(0, count)
        .map(v => v.meaning);
}

function getVerbDistractors(verb, count = 3) {
    return [...N5_VERBS]
        .filter(v => v.id !== verb.id && getStudyText(v) !== getStudyText(verb))
        .sort(() => Math.random() - 0.5)
        .slice(0, count);
}


const JAPANESE_PARTICLES = ["を", "が", "に", "で", "へ", "と", "から", "まで"];

function replaceFirstParticle(sentence, acceptedParticles, replacement) {
    // Prefer a particle listed for this verb and actually present in its example.
    const candidates = acceptedParticles
        .split("/")
        .filter(p => JAPANESE_PARTICLES.includes(p) && sentence.includes(p));
    const target = candidates[0] || JAPANESE_PARTICLES.find(p => sentence.includes(p));
    if (!target) return sentence;
    return sentence.replace(target, replacement);
}

function replaceSentenceVerb(sentence, verb, replacementForm) {
    // Replace the conjugated verb at the end while preserving punctuation.
    const punctuationMatch = sentence.match(/([。！？!?])$/);
    const punctuation = punctuationMatch ? punctuationMatch[1] : "";
    const body = punctuation ? sentence.slice(0, -1) : sentence;

    const possibleForms = [
        verb.masu,
        verb.masu.replace(/ます$/, "ました"),
        verb.masu.replace(/ます$/, "ません"),
        verb.masu.replace(/ます$/, "ませんでした"),
        verb.dictionary,
        verb.te
    ].sort((a, b) => b.length - a.length);

    for (const form of possibleForms) {
        if (body.endsWith(form)) {
            return body.slice(0, -form.length) + replacementForm + punctuation;
        }
    }

    // Fallback for sentences ending in requests such as 押してください.
    if (body.endsWith(verb.te + "ください")) {
        return body.slice(0, -(verb.te + "ください").length) + replacementForm + punctuation;
    }

    return sentence;
}

function makeSentenceGrammarOptions(verb) {
    const correct = verb.sentence;
    const accepted = verb.particle.split("/");
    const presentParticle = accepted.find(p => JAPANESE_PARTICLES.includes(p) && correct.includes(p))
        || JAPANESE_PARTICLES.find(p => correct.includes(p));
    const wrongParticles = JAPANESE_PARTICLES.filter(p => p !== presentParticle && !accepted.includes(p));

    const wrongParticle1 = wrongParticles[0] || "で";
    const wrongParticle2 = wrongParticles[1] || "が";

    const wrong1 = replaceFirstParticle(correct, verb.particle, wrongParticle1);
    const wrong2 = replaceSentenceVerb(correct, verb, verb.te);
    const wrong3 = replaceSentenceVerb(
        replaceFirstParticle(correct, verb.particle, wrongParticle2),
        verb,
        verb.dictionary
    );

    // Guarantee four distinct options. Additional fallbacks still retain the target verb.
    const candidates = [
        { text: correct, correct: true },
        { text: wrong1, correct: false },
        { text: wrong2, correct: false },
        { text: wrong3, correct: false },
        { text: replaceSentenceVerb(correct, verb, verb.masu.replace(/ます$/, "ません")), correct: false },
        { text: replaceFirstParticle(correct, verb.particle, "から"), correct: false }
    ];

    const unique = [];
    const seen = new Set();
    for (const option of candidates) {
        if (!seen.has(option.text)) {
            seen.add(option.text);
            unique.push(option);
        }
        if (unique.length === 4) break;
    }

    return unique.sort(() => Math.random() - 0.5);
}

// Start Setup on Load
window.onload = function() {
    document.getElementById('hud-total-verbs').textContent = `${N5_VERBS.length} Verbs Loaded`;
    renderVerbsTable();
    generateQuizQuestions();
    renderCurrentQuestion();
    updateHUD();
};

// Render full 100 verbs inside the interactive main view
function renderVerbsTable(filteredList = N5_VERBS) {
    const tableBody = document.getElementById('verbs-table-body');
    tableBody.innerHTML = '';

    filteredList.forEach(v => {
        const row = document.createElement('tr');
        row.className = "hover:bg-slate-50/50 transition border-b border-slate-100";
        const textDisplay = getStudyDisplay(v);

        row.innerHTML = `
            <td class="px-6 py-4 font-mono text-xs text-slate-400 font-semibold">${v.id}</td>
            <td class="px-6 py-4 font-bold text-slate-800 japanese-text text-base">${textDisplay}</td>
            <td class="px-6 py-4 text-slate-600 font-medium japanese-text">${v.dictionary}</td>
            <td class="px-6 py-4 text-slate-500 font-medium">${v.romaji}</td>
            <td class="px-6 py-4 text-slate-800 font-semibold">${v.meaning}</td>
            <td class="px-6 py-4 text-indigo-600 font-semibold japanese-text">${v.masu}</td>
            <td class="px-6 py-4 text-violet-600 font-semibold japanese-text">${v.te}</td>
            <td class="px-6 py-4"><span class="bg-slate-100 text-slate-600 px-2 py-0.5 rounded text-xs font-mono font-bold">${v.particle}</span></td>
            <td class="px-6 py-4 text-slate-500 font-medium japanese-text text-xs leading-relaxed">${v.sentence}</td>
        `;
        tableBody.appendChild(row);
    });

    const emptyState = document.getElementById('table-empty-state');
    if (filteredList.length === 0) {
        emptyState.classList.remove('hidden');
    } else {
        emptyState.classList.add('hidden');
    }
}

// Helper functions to safely find and manipulate verb phrases in sentences
function getVerbPhraseInSentence(verb) {
    const sentence = verb.sentence;
    const firstChar = verb.display.charAt(0);
    const idx = sentence.lastIndexOf(firstChar);
    if (idx !== -1) {
        return sentence.substring(idx, sentence.length - 1);
    }
    const dictFirstChar = verb.dictionary.charAt(0);
    const idx2 = sentence.lastIndexOf(dictFirstChar);
    if (idx2 !== -1) {
        return sentence.substring(idx2, sentence.length - 1);
    }
    return null;
}

function getWrongParticleSentence(verb) {
    const sentence = verb.sentence;
    const mainParticle = verb.particle.split('/')[0]; // e.g. "を", "に"
    let distractor = "に";
    
    if (mainParticle === "を") distractor = "に";
    else if (mainParticle === "に" || mainParticle === "へ") distractor = "を";
    else if (mainParticle === "で") distractor = "に";
    else if (mainParticle === "が") distractor = "を";
    
    if (sentence.includes(mainParticle)) {
        return sentence.replace(mainParticle, distractor);
    }
    return sentence.replace("を", "に").replace("に", "を"); // safe fallback
}

function getWrongMasuSentence(verb) {
    // Tests grammatical verb form conjugation (e.g. わたります vs わたるます)
    if (verb.sentence.includes(verb.masu)) {
        return verb.sentence.replace(verb.masu, verb.dictionary + "ます");
    }
    return verb.sentence.replace("ます", "るます");
}

function getWrongParticleAndMasuSentence(verb) {
    // Simultaneously tests both the wrong preposition/particle AND the wrong verb form
    let sentence = getWrongParticleSentence(verb);
    if (sentence.includes(verb.masu)) {
        return sentence.replace(verb.masu, verb.dictionary + "ます");
    }
    return sentence.replace("ます", "るます");
}

// Guarantee 4 distinct choices every time, with fallback generation
function makeOptionsUnique(options, correctAnswer, verb, type) {
    const uniqueSet = new Set();
    const result = [];
    for (let opt of options) {
        const text = opt.text.trim();
        if (!uniqueSet.has(text) && text !== "") {
            uniqueSet.add(text);
            result.push(opt);
        }
    }
    while (result.length < 4) {
        let randomVerb = verb;
        while (randomVerb.id === verb.id) {
            randomVerb = N5_VERBS[Math.floor(Math.random() * N5_VERBS.length)];
        }
        let fillText = "";
        if (type === 'masu') {
            fillText = randomVerb.masu;
        } else if (type === 'sentence') {
            const verbPhrase = getVerbPhraseInSentence(verb);
            if (verbPhrase) {
                fillText = verb.sentence.replace(verbPhrase, randomVerb.masu);
            } else {
                fillText = randomVerb.sentence;
            }
        } else {
            fillText = randomVerb.masu;
        }
        if (!uniqueSet.has(fillText)) {
            uniqueSet.add(fillText);
            result.push({ text: fillText, correct: false });
        }
    }
    return result.sort(() => Math.random() - 0.5);
}

// Generates realistic grammatical errors of the SAME verb to test actual conjugation knowledge
function getIncorrectMasuForms(verb) {
    const correct = verb.masu; // e.g. "いきます"
    const dictionary = verb.dictionary; // e.g. "いく"
    const distractors = new Set();

    // 1. Dictionary form + ます (e.g. "いくます", "たべるます")
    distractors.add(dictionary + "ます");

    // Special handling for N5 Irregular Verbs
    if (dictionary === "くる" || verb.display === "来る") {
        return ["くるます", "くります", "こます"];
    }
    if (dictionary === "する") {
        return ["するます", "すります", "せます"];
    }

    // 2. Godan/Ichidan Swap Mistake:
    if (dictionary.endsWith("る")) {
        const base = dictionary.slice(0, -1); // e.g. "たべ" from "たべる"
        if (correct === base + "ます") {
            // Ichidan verb conjugated like a Godan verb (extremely common beginner error!)
            distractors.add(base + "ります"); // "たべります"
            distractors.add(base + "られます"); // "たべられます"
        } else {
            // Godan verb ending in -ru (like のぼる -> のぼります) conjugated like Ichidan (dropping 'ru')
            distractors.add(base + "ます"); // "のぼます"
        }
    }

    // 3. Vowel column shift mistakes (modifying the kana stem before ~masu)
    const stem = correct.slice(0, -2); // remove "ます" -> e.g. "いき", "たべ", "のみ"
    if (stem.length > 0) {
        const lastChar = stem.slice(-1); // "き", "べ", "み"
        const baseStem = stem.slice(0, -1); // "い", "た", "の"

        const rows = {
            'き': ['か', 'け', 'く'],
            'ぎ': ['が', 'げ', 'ぐ'],
            'し': ['さ', 'せ', 'す'],
            'ち': ['た', 'て', 'つ'],
            'に': ['な', 'ね', 'ぬ'],
            'ひ': ['は', 'へ', 'ふ'],
            'び': ['ば', 'べ', 'ぶ'],
            'み': ['ま', 'め', 'む'],
            'り': ['ら', 'れ', 'る'],
            'い': ['わ', 'え', 'う'],
            'べ': ['ば', 'び', 'ぶ'],
            'ま': ['み', 'め', 'む'],
            'お': ['あ', 'い', 'う']
        };

        const replacements = rows[lastChar];
        if (replacements) {
            replacements.forEach(r => {
                distractors.add(baseStem + r + "ます");
            });
        }
    }

    // 4. Common safety fallbacks if set size is too small
    const safetyFillers = ["みます", "します", "きます", "ります", "ちます"];
    let i = 0;
    while (distractors.size < 5 && i < safetyFillers.length) {
        const stem = correct.slice(0, -2);
        if (stem.length > 0) {
            distractors.add(stem.slice(0, -1) + safetyFillers[i]);
        }
        i++;
    }

    // Ensure the correct answer is not present in distractors
    distractors.delete(correct);

    return Array.from(distractors).slice(0, 3);
}

// Generate unique quiz questions strictly with N5 compliant formats and smart same-verb distractors
function generateQuizQuestions() {
    state.questions = [];
    const shuffled = [...N5_VERBS].sort(() => Math.random() - 0.5);
    const total = Math.min(QUIZ_LENGTH, shuffled.length);

    for (let i = 0; i < total; i++) {
        const verb = shuffled[i];
        const mode = i % 5;

        if (mode === 0) {
            const distractors = [...new Set([
                verb.dictionary + "て",
                verb.masu.replace(/ます$/, "て"),
                verb.dictionary.replace(/る$/, "") + "って",
                ...N5_VERBS
                    .filter(v => v.id !== verb.id)
                    .sort(() => Math.random() - 0.5)
                    .slice(0, 5)
                    .map(v => v.te)
            ])].filter(x => x && x !== verb.te).slice(0, 3);

            const options = [
                { text: verb.te, correct: true },
                ...distractors.map(text => ({ text, correct: false }))
            ].sort(() => Math.random() - 0.5);

            state.questions.push({
                type: "choice",
                verb,
                prompt: `What is the correct <strong>て-form</strong> of ${getStudyDisplay(verb)} (${verb.meaning})?`,
                options,
                correctAnswer: verb.te
            });
        } else if (mode === 1) {
            const options = [
                { text: verb.meaning, correct: true },
                ...getMeaningDistractors(verb).map(text => ({ text, correct: false }))
            ].sort(() => Math.random() - 0.5);

            state.questions.push({
                type: "choice",
                verb,
                prompt: `What does ${getStudyDisplay(verb)} mean?`,
                options,
                correctAnswer: verb.meaning
            });
        } else if (mode === 2) {
            const options = [
                { text: getStudyText(verb), correct: true },
                ...getVerbDistractors(verb).map(v => ({
                    text: getStudyText(v),
                    correct: false
                }))
            ].sort(() => Math.random() - 0.5);

            state.questions.push({
                type: "choice",
                verb,
                prompt: `Which Japanese verb means <strong>${verb.meaning}</strong>?`,
                options,
                correctAnswer: getStudyText(verb)
            });
        } else if (mode === 3) {
            const distractors = getIncorrectMasuForms(verb);
            const options = [
                { text: verb.masu, correct: true },
                ...distractors.map(text => ({ text, correct: false }))
            ].sort(() => Math.random() - 0.5);

            state.questions.push({
                type: "choice",
                verb,
                prompt: `What is the polite <span class="text-indigo-600">ます-form</span> of ${getStudyDisplay(verb)} (${verb.meaning})?`,
                options,
                correctAnswer: verb.masu
            });
        } else {
            const options = makeSentenceGrammarOptions(verb);
            state.questions.push({
                type: "choice",
                verb,
                prompt: `Choose the grammatically correct sentence using ${getStudyDisplay(verb)}. Pay attention to the <strong>particle and verb form</strong>.`,
                options,
                correctAnswer: verb.sentence
            });
        }
    }
}

function renderCurrentQuestion() {
    const currentQ = state.questions[state.currentQuestionIndex];
    if (!currentQ) return;

    // Update index labels
    document.getElementById('quiz-question-index').textContent = `Question ${state.currentQuestionIndex + 1} of ${state.questions.length}`;
    document.getElementById('quiz-prompt').innerHTML = currentQ.prompt;

    const optContainer = document.getElementById('quiz-options-container');
    const typedContainer = document.getElementById('quiz-text-entry-container');
    const badgeType = document.getElementById('quiz-badge-type');

    // Check if this question was already answered (User is reviewing)
    const savedAnswer = state.userAnswers[state.currentQuestionIndex];
    const isQCorrect = state.isCorrect[state.currentQuestionIndex];

    // Clean inputs and view blocks
    optContainer.innerHTML = '';
    
    if (currentQ.type === 'typed') {
        badgeType.textContent = "Typed Input Challenge";
        badgeType.className = "w-fit px-2 py-1 bg-amber-50 text-amber-700 rounded text-[10px] font-bold tracking-wider uppercase";
        optContainer.classList.add('hidden');
        typedContainer.classList.remove('hidden');

        const typedInput = document.getElementById('typed-answer');
        const typedBtn = document.getElementById('btn-typed-submit');
        
        if (savedAnswer !== null) {
            typedInput.value = savedAnswer;
            typedInput.disabled = true;
            typedBtn.disabled = true;
            typedBtn.className = "bg-slate-300 text-slate-500 px-6 py-3 rounded-xl font-semibold cursor-not-allowed";
        } else {
            typedInput.value = '';
            typedInput.disabled = false;
            typedBtn.disabled = false;
            typedBtn.className = "bg-indigo-600 hover:bg-indigo-500 text-white px-6 py-3 rounded-xl font-semibold transition flex items-center gap-2";
        }
    } else {
        badgeType.textContent = "Multiple Choice Challenge";
        badgeType.className = "w-fit px-2 py-1 bg-indigo-50 text-indigo-700 rounded text-[10px] font-bold tracking-wider uppercase";
        optContainer.classList.remove('hidden');
        typedContainer.classList.add('hidden');

        currentQ.options.forEach(opt => {
            const btn = document.createElement('button');
            const isCorrectOption = (opt.text.trim() === currentQ.correctAnswer.trim());
            const isSelectedOption = (savedAnswer !== null && opt.text.trim() === savedAnswer.trim());

            if (savedAnswer !== null) {
                // Read-only state styles for completed questions
                if (isCorrectOption) {
                    btn.className = "bg-emerald-50 border-2 border-emerald-500 text-emerald-800 font-semibold px-4 py-3.5 rounded-xl text-left flex items-center justify-between text-sm shadow-sm cursor-default w-full";
                    btn.innerHTML = `
                        <span class="japanese-text">${opt.text}</span>
                        <span class="material-icons text-emerald-600 text-sm">check_circle</span>
                    `;
                } else if (isSelectedOption) {
                    btn.className = "bg-rose-50 border-2 border-rose-300 text-rose-800 font-semibold px-4 py-3.5 rounded-xl text-left flex items-center justify-between text-sm shadow-sm cursor-default w-full";
                    btn.innerHTML = `
                        <span class="japanese-text">${opt.text}</span>
                        <span class="material-icons text-rose-600 text-sm">cancel</span>
                    `;
                } else {
                    btn.className = "bg-slate-50 border border-slate-100 text-slate-400 px-4 py-3.5 rounded-xl text-left flex items-center justify-between text-sm w-full opacity-60 cursor-default";
                    btn.innerHTML = `<span class="japanese-text">${opt.text}</span>`;
                }
                btn.disabled = true;
            } else {
                // Interactive active state style
                btn.className = "bg-white hover:bg-slate-50 border border-slate-200 hover:border-indigo-300 text-slate-700 font-medium px-4 py-3.5 rounded-xl text-left transition flex items-center justify-between text-sm shadow-sm group duration-150 w-full";
                btn.innerHTML = `
                    <span class="japanese-text">${opt.text}</span>
                    <span class="material-icons opacity-0 group-hover:opacity-100 text-indigo-400 text-sm transition">arrow_forward</span>
                `;
                btn.onclick = () => handleChoiceAnswer(opt.text);
            }
            optContainer.appendChild(btn);
        });
    }

    // Handle feedback notice status display
    const alertBox = document.getElementById('feedback-alert');
    const alertIcon = document.getElementById('feedback-icon');
    const alertText = document.getElementById('feedback-text');

    if (savedAnswer !== null) {
        alertBox.classList.remove('hidden');
        if (isQCorrect) {
            alertBox.className = "flex items-center gap-3 p-4 rounded-xl border border-emerald-200 bg-emerald-50 text-emerald-800 font-medium";
            alertIcon.textContent = "check_circle";
            alertText.textContent = "Perfect! Correct response. 🌸";
        } else {
            alertBox.className = "flex items-center gap-3 p-4 rounded-xl border border-rose-200 bg-rose-50 text-rose-800 font-medium";
            alertIcon.textContent = "cancel";
            alertText.textContent = `Incorrect response. Expected N5 Answer: ${currentQ.correctAnswer}`;
        }
    } else {
        alertBox.classList.add('hidden');
    }

    // Refresh Navigation buttons
    const prevBtn = document.getElementById('btn-prev-question');
    const nextBtn = document.getElementById('btn-next-question');
    
    prevBtn.disabled = (state.currentQuestionIndex === 0);
    // User can only press 'Next' if they have already provided an answer to the active question
    nextBtn.disabled = (savedAnswer === null);

    if (state.currentQuestionIndex === state.questions.length - 1) {
        nextBtn.innerHTML = `Finish <span class="material-icons text-sm">assignment_turned_in</span>`;
    } else {
        nextBtn.innerHTML = `Next <span class="material-icons text-sm">arrow_forward</span>`;
    }

    // Refresh UI progress bar
    const progressPercent = ((state.currentQuestionIndex + 1) / state.questions.length) * 100;
    document.getElementById('quiz-progress-bar').style.width = `${progressPercent}%`;
    document.getElementById('quiz-progress-text').textContent = `${Math.round(progressPercent)}% Complete`;
}

// Navigation back function
function goToPrevQuestion() {
    if (state.advanceTimeout) {
        clearTimeout(state.advanceTimeout);
        state.advanceTimeout = null;
    }
    if (state.currentQuestionIndex > 0) {
        state.currentQuestionIndex--;
        renderCurrentQuestion();
        updateHUD();
    }
}

// Navigation forward function
function goToNextQuestion() {
    if (state.advanceTimeout) {
        clearTimeout(state.advanceTimeout);
        state.advanceTimeout = null;
    }
    if (state.userAnswers[state.currentQuestionIndex] !== null) {
        if (state.currentQuestionIndex >= 9) {
            renderFinishedState();
        } else {
            state.currentQuestionIndex++;
            renderCurrentQuestion();
            updateHUD();
        }
    }
}

// Process Choice Response
function handleChoiceAnswer(selectedText) {
    if (state.feedback || state.userAnswers[state.currentQuestionIndex] !== null) return;
    
    const currentQ = state.questions[state.currentQuestionIndex];
    const isCorrect = (selectedText.trim() === currentQ.correctAnswer.trim());
    processFeedback(isCorrect, currentQ.correctAnswer, selectedText);
}

// Process Typed response
function submitTypedResponse() {
    if (state.feedback || state.userAnswers[state.currentQuestionIndex] !== null) return;
    
    const inputVal = document.getElementById('typed-answer').value.trim();
    if (!inputVal) return;

    const currentQ = state.questions[state.currentQuestionIndex];
    const isCorrect = (inputVal.toLowerCase() === currentQ.correctAnswer.toLowerCase());
    processFeedback(isCorrect, currentQ.correctAnswer, inputVal);
}

function processFeedback(isCorrect, correctDisplay, selectedText) {
    if (state.advanceTimeout) {
        clearTimeout(state.advanceTimeout);
        state.advanceTimeout = null;
    }

    state.userAnswers[state.currentQuestionIndex] = selectedText;
    state.isCorrect[state.currentQuestionIndex] = isCorrect;
    
    // Recalculate score from stored answers
    state.score = state.isCorrect.filter(x => x === true).length;

    updateHUD();
    renderCurrentQuestion();

    // Stay on this question so the learner can review the feedback.
    // The Next button controls progression.
}

function setAppView(viewName) {
    state.view = viewName;
    const qView = document.getElementById('view-quiz');
    const tView = document.getElementById('view-table');

    const qBtn = document.getElementById('btn-quiz-mode');
    const tBtn = document.getElementById('btn-table-mode');

    if (viewName === 'quiz') {
        qView.classList.remove('hidden');
        tView.classList.add('hidden');
        qBtn.className = "px-4 py-2 rounded-lg bg-indigo-800 text-white font-semibold text-sm hover:bg-indigo-900 transition flex items-center gap-2";
        tBtn.className = "px-4 py-2 rounded-lg bg-indigo-600 text-indigo-100 hover:text-white font-semibold text-sm hover:bg-indigo-500 transition flex items-center gap-2";
    } else {
        qView.classList.add('hidden');
        tView.classList.remove('hidden');
        tBtn.className = "px-4 py-2 rounded-lg bg-indigo-800 text-white font-semibold text-sm hover:bg-indigo-900 transition flex items-center gap-2";
        qBtn.className = "px-4 py-2 rounded-lg bg-indigo-600 text-indigo-100 hover:text-white font-semibold text-sm hover:bg-indigo-500 transition flex items-center gap-2";
    }
    updateHUD();
}

// Update Stats banner HUD
function updateHUD() {
    document.getElementById('hud-mode').textContent = state.view === 'quiz' ? 'Interactive Quiz' : 'Reference Guide';
    
    // Total completed questions
    const answeredCount = state.userAnswers.filter(x => x !== null).length;
    document.getElementById('hud-score').textContent = `${state.score} / ${answeredCount}`;
}

// Reset Quiz State
function resetQuiz() {
    if (state.advanceTimeout) {
        clearTimeout(state.advanceTimeout);
        state.advanceTimeout = null;
    }
    state.currentQuestionIndex = 0;
    state.score = 0;
    state.quizFinished = false;
    state.feedback = null;
    state.userAnswers = Array(QUIZ_LENGTH).fill(null);
    state.isCorrect = Array(QUIZ_LENGTH).fill(null);
    generateQuizQuestions();
    renderCurrentQuestion();
    updateHUD();
}

// Render Finished Screen
function renderFinishedState() {
    const optContainer = document.getElementById('quiz-options-container');
    const typedContainer = document.getElementById('quiz-text-entry-container');
    const badgeType = document.getElementById('quiz-badge-type');

    badgeType.textContent = "Quiz Finished";
    badgeType.className = "w-fit px-2 py-1 bg-emerald-100 text-emerald-800 rounded text-[10px] font-bold tracking-wider uppercase";

    optContainer.classList.add('hidden');
    typedContainer.classList.add('hidden');

    const scorePct = (state.score / 10) * 100;
    let rankMsg = "Keep practicing! 頑張ってください！";
    if (scorePct >= 90) rankMsg = "Excellent! Perfect understanding! 素晴らしい！";
    else if (scorePct >= 70) rankMsg = "Great job! Very solid performance! よくできました！";

    document.getElementById('quiz-prompt').innerHTML = `
        <div class="text-center py-6 flex flex-col items-center gap-4 animate-in fade-in zoom-in duration-300">
            <span class="material-icons text-emerald-500 text-6xl">emoji_events</span>
            <div>
                <h3 class="text-2xl font-bold text-slate-800">You Finished the Quiz!</h3>
                <p class="text-sm text-slate-500 mt-1">${rankMsg}</p>
            </div>
            <div class="bg-indigo-50 border border-indigo-100 rounded-2xl px-8 py-4 my-2">
                <span class="text-slate-500 text-xs uppercase tracking-wider font-semibold block">Final Score</span>
                <span class="text-4xl font-extrabold text-indigo-700">${state.score} <span class="text-xl font-normal text-slate-400">/ 10</span></span>
            </div>
            <button onclick="resetQuiz()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-6 py-3 rounded-xl font-semibold transition flex items-center gap-2">
                <span class="material-icons text-sm">replay</span> Play Again
            </button>
        </div>
    `;
    
    // Hide previous/next buttons on finish state
    document.getElementById('btn-prev-question').classList.add('hidden');
    document.getElementById('btn-next-question').classList.add('hidden');
}

// Toast notification trigger
function showToast(message, isError = false) {
    const banner = document.getElementById('toast-banner');
    const icon = document.getElementById('toast-icon');
    const text = document.getElementById('toast-text');

    text.textContent = message;
    if (isError) {
        icon.textContent = 'error';
        icon.className = 'material-icons text-rose-400';
    } else {
        icon.textContent = 'check_circle';
        icon.className = 'material-icons text-emerald-400';
    }

    banner.classList.remove('translate-y-20', 'opacity-0', 'pointer-events-none');
    banner.classList.add('translate-y-0', 'opacity-100');

    setTimeout(() => {
        banner.classList.remove('translate-y-0', 'opacity-100');
        banner.classList.add('translate-y-20', 'opacity-0', 'pointer-events-none');
    }, 3000);
}

// Search Filter function
function handleTableSearch(term) {
    const sanitized = term.toLowerCase().trim();
    if (!sanitized) {
        renderVerbsTable(N5_VERBS);
        return;
    }

    const filtered = N5_VERBS.filter(v => {
        return v.display.toLowerCase().includes(sanitized) ||
               v.dictionary.toLowerCase().includes(sanitized) ||
               v.romaji.toLowerCase().includes(sanitized) ||
               v.meaning.toLowerCase().includes(sanitized) ||
               v.masu.toLowerCase().includes(sanitized) ||
               v.te.toLowerCase().includes(sanitized) ||
               v.sentence.toLowerCase().includes(sanitized);
    });

    renderVerbsTable(filtered);
}

// Generates and triggers browser download for full 100 verb list
function downloadCSV() {
    let csvContent = "data:text/csv;charset=utf-8,";
    csvContent += "ID,Study Display,Hiragana,Romaji,Polite form (~masu),Te-form,Particle,English Meaning,Example Sentence\n";
    
    N5_VERBS.forEach(v => {
        const row = [
            v.id,
            getStudyText(v),
            v.dictionary,
            v.romaji,
            v.masu,
            v.te,
            v.particle,
            `"${v.meaning.replace(/"/g, '""')}"`,
            `"${v.sentence.replace(/"/g, '""')}"`
        ];
        csvContent += row.join(",") + "\n";
    });

    const encodedUri = encodeURI(csvContent);
    const link = document.createElement("a");
    link.setAttribute("href", encodedUri);
    link.setAttribute("download", "JLPT_N5_100_Verbs.csv");
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    showToast("CSV file download initiated successfully!");
}

// Copies CSV list to clipboard securely (respects prompt CSP/no alert directives)
function copyCSVToClipboard() {
    let csvData = "ID,Study Display,Hiragana,Romaji,Polite form (~masu),Te-form,Particle,English Meaning,Example Sentence\r\n";
    
    N5_VERBS.forEach(v => {
        const row = [
            v.id,
            getStudyText(v),
            v.dictionary,
            v.romaji,
            v.masu,
            v.te,
            v.particle,
            `"${v.meaning.replace(/"/g, '""')}"`,
            `"${v.sentence.replace(/"/g, '""')}"`
        ];
        csvData += row.join(",") + "\r\n";
    });

    const tempTextArea = document.createElement("textarea");
    tempTextArea.value = csvData;
    document.body.appendChild(tempTextArea);
    tempTextArea.select();
    try {
        document.execCommand('copy');
        showToast("Success! Copied CSV Dataset to clipboard.");
    } catch (err) {
        showToast("Clipboard copy failed. Please use download button.", true);
    }
    document.body.removeChild(tempTextArea);
}

// Open beautiful overlay modal of printable study card dataset
function openPrintFriendly() {
    const modal = document.getElementById('print-modal');
    modal.classList.remove('hidden');

    const printBody = document.getElementById('print-table-body');
    printBody.innerHTML = '';

    N5_VERBS.forEach(v => {
        const tr = document.createElement('tr');
        tr.className = "hover:bg-slate-50";
        tr.innerHTML = `
            <td class="px-2 py-1.5 border border-slate-300 font-mono text-[10px] text-center font-bold">${v.id}</td>
            <td class="px-2 py-1.5 border border-slate-300 font-extrabold text-slate-900 text-sm japanese-text">${getStudyDisplay(v)}</td>
            <td class="px-2 py-1.5 border border-slate-300 text-slate-800 text-xs japanese-text">${v.dictionary}</td>
            <td class="px-2 py-1.5 border border-slate-300 text-slate-600 text-xs">${v.romaji}</td>
            <td class="px-2 py-1.5 border border-slate-300 text-indigo-700 font-bold text-xs japanese-text">${v.masu}</td>
            <td class="px-2 py-1.5 border border-slate-300 text-violet-700 font-bold text-xs japanese-text">${v.te}</td>
            <td class="px-2 py-1.5 border border-slate-300 text-center text-xs font-bold text-slate-600">${v.particle}</td>
            <td class="px-2 py-1.5 border border-slate-300 text-slate-700 text-xs">${v.meaning}</td>
            <td class="px-2 py-1.5 border border-slate-300 text-slate-600 text-[11px] japanese-text">${v.sentence}</td>
        `;
        printBody.appendChild(tr);
    });
}

// Close printer friendly view
function closePrintFriendly() {
    document.getElementById('print-modal').classList.add('hidden');
}

// Print trigger window function
function triggerPrintWindow() {
    const printContent = document.getElementById('print-view-document').innerHTML;

    // Open a temporary printing window
    const printWindow = window.open('', '_blank', 'height=600,width=800');
    printWindow.document.write('<html><head><title>JLPT N5 strict 100 Verbs reference sheet</title>');
    printWindow.document.write('<style>');
    printWindow.document.write('body { font-family: sans-serif; padding: 20px; color: #111827; }');
    printWindow.document.write('table { width: 100%; border-collapse: collapse; margin-top: 15px; }');
    printWindow.document.write('th, td { border: 1px solid #9ca3af; padding: 8px; text-align: left; font-size: 11px; }');
    printWindow.document.write('th { background-color: #f3f4f6; font-weight: bold; }');
    printWindow.document.write('.text-center { text-align: center; }');
    printWindow.document.write('.font-bold { font-weight: bold; }');
    printWindow.document.write('.text-indigo-700 { color: #4338ca; }');
    printWindow.document.write('.border-b-2 { border-bottom: 2px solid #e5e7eb; }');
    printWindow.document.write('</style></head><body>');
    printWindow.document.write(printContent);
    printWindow.document.write('</body></html>');
    printWindow.document.close();
    printWindow.focus();
    setTimeout(() => {
        printWindow.print();
        printWindow.close();
    }, 250);
}

// Simple direct print trigger for standard page print
function printTableData() {
    openPrintFriendly();
    setTimeout(() => {
        triggerPrintWindow();
    }, 200);
}
</script>
</html>
