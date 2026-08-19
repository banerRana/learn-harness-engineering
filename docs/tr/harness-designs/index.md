# Öncü Harness Tasarımlarının İncelenmesi

Bu bölüm, ders notlarında ele alınan harness teorisini günümüzün en ileri gerçek ürünleriyle tek tek karşılaştırır. Her üründe yalnızca tek bir şeye odaklanıyoruz: **harness nasıl tasarlanmış** — yani modelin çevresindeki mühendislik altyapısına: talimatlar, araçlar, ortam, durum ve geri bildirimden oluşan beş alt sisteme; ayrıca bağlam sürekliliği, başlatma, doğrulama, gözlemlenebilirlik, devir ve döngü gibi temel mekanizmalara.

Modelin muhakeme gücünün ne kadar yüksek olduğunu, belirli bir benchmark sonucunu ya da genel olarak "bu agent ne yapabilir" sorusunu özellikle ele almıyoruz. Bunlar model ve ürün katmanlarının konularıdır. Burada yalnızca harness'ı, yani model ağırlıklarının dışındaki her şeyi inceliyoruz.

## Neden İncelemeye Değer

Ders notlarının ilk bölümünde belirtildiği gibi, güçlü bir model güvenilir yürütme anlamına gelmez. Aynı model farklı harness'larda kullanıldığında performansı bir büyüklük mertebesi kadar değişebilir. Ancak ders notları "nasıl yapılmalı" sorusunu yanıtlarken bu ürünler, "önde gelen ekipler bunu gerçekte nasıl yapıyor" sorusunu yanıtlar.

Her ürün bağımsız bir tasarım kararları bütünüdür. Bunları yan yana koyduğunuzda aynı temel mekanizmaların farklı ekipler tarafından bütünüyle farklı biçimlerde hayata geçirildiğini görürsünüz:

- **Pi**, harness'ı son derece yalın bir çekirdek + programlanabilir Extensions olarak kurar ve "minimum sistem istemi + gerektiğinde yükleme" yaklaşımıyla bağlam mühendisliği yapar.
- **Claude Code**, harness'ı eksiksiz bir çalışma ortamı olarak kurar: katmanlı bellek, beş aşamalı compaction, permissions, hooks ve subagent'lar.
- **Codex**, harness felsefesini en uç noktaya taşır: gerçekliğin kaynağı depodur, AGENTS.md yalnızca bir dizin sayfasıdır ve ortam worktree ile yalıtılır.
- **DeepSeek Harness**, harness'ın kendisini modelden bağımsız bir runtime olarak tanımlar: Everything is a Plugin.

## Yazı Listesi

- [Pi harness tasarımının incelenmesi](./pi/): Yalın çekirdek + programlanabilir Extensions; bağlam mühendisliğini sistem isteminin dışına taşır.
- [Claude Code harness tasarımının incelenmesi](./claude-code/): Katmanlı bellek, beş aşamalı compaction, permissions ve hooks ile eksiksiz bir agent çalışma ortamı.
- [Codex harness tasarımının incelenmesi](./codex/): Gerçekliğin kaynağı depo, AGENTS.md bir dizin sayfası; ortam yalıtımı ve geri bildirim döngüsü.
- [DeepSeek Harness tasarımının incelenmesi](./deepseek/): Everything is a Plugin; agent döngüsünün kendisini değiştirilebilir bir plugin'e dönüştürür.

## Nasıl Okunmalı

Önce beş alt sistem çerçevesini oluşturmak için ders notlarının ilk bölümlerini (özellikle [Ders 2: Harness Tam Olarak Nedir?](../lectures/lecture-02-what-a-harness-actually-is/)) okumanızı, ardından gerçek ürünlerin bu mekanizmaları nasıl hayata geçirdiğini görmek için buraya dönmenizi öneririz.

Her yazının sonunda, ürün tasarımını hızla ders kavramlarına çevirmenize ve doğrudan kendi projenize uyarlamanıza yardımcı olan "Ders Çerçevesiyle Eşleştirme" ve "Örnek Alınabilecek Tasarımlar" bölümleri bulunur.
