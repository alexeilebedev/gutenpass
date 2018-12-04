To generate a few random passwords, run `./fewpass`
These are built with help of /dev/random and will be different
each time.

To convert a number to a mnemonic string, use `./gutenpass <number>`
See `./gutenpass -h` for help.

By default, gutenpass picks a random lang file, but this can be customized
with -langfile option.

## Lang file
The lang file is a list of syllable-to-syllable transitions.
Word boundary is represented as -.
Here is a complete simple langfile (lang/blah.lang):
~~~
-: bl gl fl
bl: a e i o u
gl: a e i o u
fl: a e i o u
a: r g h s t
e: r g h s t 
i: r g h s t 
o: r g h s t 
u: r g h s t 
r: a e i o u -
g: a e i o u -
h: a e i o u -
s: a e i o u -
t: ie -
~~~

This says that a word can start with bl, gl or fl.
bl can be followed by a e i o, or u. etc.
The file generates phrases like this:
~~~
    bligigasutie-glutie-flit-glahage
    flotie-blirut-flugugohaguru
    fluhuguh-flagohut-glesut
    blehagigatie-blagetie-bluhuh-
    flat-blehesirogugerat-glu
~~~
The fanout for each syllable here is rather small, so the resulting
strings end up being long, since each syllable encodes only a few bits of information.
Good (and fun) langfiles result from analysis of texts.

## Provided lang files
A dozen lang files are already provided. These include War And Peace,
Critique of Pure Reason, Great Expectations, Alice in Wonderland,
and other classics. Texts are in txt/ and resulting langfiles are under lang.
Lang files can be re-generated with ./update-lang (read the source for more).

~~~
$ ./gutenpass -l lang/purereason.lang -n 10 -t 0 -minfreq 40
trandere-d-sp-hr-brin-fr
habewennt-hr-d-syntalen-g
etwahr-gleicht-kannen-wirklichti
sinden-v-blo-sofern-hes-do
dies-gleicht-tischon-ere-ck
i-sicht-trann-bleigegeht
gleigen-jederzeinand-ner-da
sofern-notwedere-hr-hin-woll
jederzeinem-nken-werd-ohnes-ab
r-l-bi-nz-re-suchti
~~~

## Tutorial
To create your own language file
and generate a random string from it. We'll use Cervantes' Don Quijote,
in original spanish, as input:
~~~
curl -s http://www.gutenberg.org/cache/epub/2000/pg2000.txt > x
./makelang x -token digram -minfreq 10 -minword 5 > y
./gutenpass -l y -n 5 -t 0
~~~
This would produce something like the following:
~~~
opelendoler-chusmatr
experoncelosasimonio-
bebeletande-brardo
talegunanderr-doso
yegurinegro-pasesenv
~~~

## Different source languages
It is fun to train gutenpass on texts in languages other than English.
The source language flavor clearly shows through in the resulting strings.
~~~
./gutenpass -l lang/purereason.lang -n 10 -t 0
ame-kurzt-oniellespo
ihenberiecherke-eg
jetzliebillesendigzufa
hrt-usucht-af-ume
kurzt-ttis-am-z-ad
ewahndliebigt-bractu
utertiers-bleinzuk
www-ma-ihneindiativ
hmentbart-katehlt-fu
sodividividikate-folgem
~~~

## Makelang
This creates a lang file out of a txt file. 
This consists of cleaning the input file (discarding words containing
uppercase characters, and short lines), and then breaking the words
up into tokens. Three token methods are supported:

vowel    Divide text into runs of consecutive vowels / consonants.
digram   Each token is 2 consecutive characters
trigram  Each token is 3 consecutive characters

The trigram method generates the most realistic strings, but they will 
be long, because resulting entropy is low. Vowel method generates words that
are quite memorable, and have high entropy (only brute force will break
suck passwords; they are immunte to dictionary attack.
Digram method is somewhere in between.

By default, makelang assumes Project Gutenberg files,
meaning that anything before the words 'START OF THE PROJECT GUTENBERG ...'
is ignored. -no-guten can be specified if passing arbitrary text.

## About Entropy
When I'm talking about the amount of entropy in a string, I'm referring
to the size of the smallest space from which the string can be recalculated.
For instance, if you call srand(time(0)) and then generate
a 128-character string, the resulting entropy would be only 25 bits or so, because the number
of possible strings would be equal to the number of seconds in a year.

Gutenpass by default generates passwords containing 48 bits of entropy
sampled from /dev/random. This may seem like a low number, but it is
practical: the resulting strings often have 70 bits of information and more, lang files
can be customized, and each gutenpass run has enough options to make
running gutenpass 2^48 times impractical. Plus it's slow :-)
