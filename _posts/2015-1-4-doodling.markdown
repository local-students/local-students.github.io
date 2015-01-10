---
layout: post
title:週一で何か書いてみよう企画
date:2015-1-4 20:00:00
categories: doodling
---

###はじめに

こんばんは。そしてあけましておめでとうございます。
[canonxex](https://twitter.com/HOMOMAID)です。

本ブログの読者の方々は、週一で何か書いてみよう企画をまだ覚えていましたでしょうか？

覚えていた方は大幅に遅れてごめんなさい。
忘れていた方はごめんなさい、忘れるほど間を開けてしまいました。

今回、技術系の記事を書くという機会を始めていただいたのですが、中々ネタが決まらず、構想が浮かんでは崩れ、浮かんでは崩れ…と思考錯誤していたうちに大幅に遅れてしまいました。

物書き気取りの謝罪はこのくらいにして。

長い時間を頂いて結局何が完成したのかこの記事の読者にご覧いただこうかと思います。

###アプリケーション名「SigntoMd4MTG」(長い)

まず、我々LOCAL学生部は、[SIGN](https://svgn.biz/)という議事録管理サービスを利用しながらMTGをしています。
本ブログに少しでも目を通していただければお分かりになるでしょうが、こちらのブログでもMTGの議事録を紹介していますよね。
SIGNというサービスでは、アプリケーション上で作成した議事録をテキストファイルに出力することができるのですが…

**恐ろしいほどこのテキストファイルのフォーマットが扱いづらいのです。**

具体的にどのような点が扱いづらいのかというと、

- 全角スペースが多用されている
- 本ブログはmdで書かれているが、それに合わないフォーマットが多々ある
	- "-"の代わりに"・"を使用している
	- etc...

などといった点です。

手動で編集するとすごく面倒くさいところが多く、実はSIGNでの議事録をブログにアップロードする作業を担当したがる人は学生部内でも皆無といった状況です。
~~もっと使いやすい議事録管理ツールを使った方がいいんだろうか~~

しかし、逆に言えば結構自動化できる扱いづらいポイントが多いのも事実。
簡単な置換だったりで作業は楽になると僕は考えました。

自動化…ということでプログラミングの出番。
というわけで簡単にコーディングしました。

使用言語はruby。開発時間は多分2時間弱程度です。

```ruby
require 'date'

def checklinesnumber(lines)
	Integer(lines[0])
	true

	rescue ArgumentError
	false
end

puts "Please input the file title"
title = gets.chomp
puts "Please input the file id"
id = gets.chomp

inputfilename = title +  "_" + id + ".txt"
newfilename = Date::today.to_s + "-MTG.markdown"

File.open(newfilename, "a") do |nf| 
	nf.puts("---\n")
	nf.puts("layout: post\n")
	nf.puts("title: MTGの報告です\n")
	nf.puts("date:" + Time.now.strftime("%Y-%m-%d %H:%M:%S") + "\n")
	nf.puts("categories: MTG\n")
	nf.puts("---\n\n")
	nf.puts("#" + Time.now.strftime("%m/%d") + " MTG 議事録\n\n")
end

File.open(inputfilename, "r") do |f|
	while lines = f.gets
		if checklinesnumber(lines)
			lines.insert(0, "#")
			lines.insert(2, ".")
		end
		lines.gsub!("　", " ")
		lines.gsub!("・", "-")

		File.open(newfilename, "a") do |nf| 
  			nf.rewind
  			nf.puts(lines)
		end

		puts lines
	end
end
```

やったことはソースコードの通りなのですが、簡単に言えば文章を決まったルールで決まった単語をまた違う決まった単語に置換したり、決まった単語を挿入したりしました。

これで少しはブログの記事数も増えるのかな…？
ま、まぁ本当はこの企画みたいにコンテンツめいた記事をアップロードしたほうが面白いんだろうけど！(汗)

というわけで次回は遅くならないように頑張ります…
ありがとうございました。
