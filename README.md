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
Iliad, Great Expectations, Alice in Wonderland,
and other classics. Texts are in txt/ and resulting langfiles are under lang.
Lang files can be re-generated with ./update-lang (read the source for more).

Here a few passphrases inspired by Critique of Pure Reason:
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
Let's create a new language file and generate a random string from it.
We'll use Cervantes' Don Quijote, in original Spanish, as input:
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
Work consists of cleaning the input file (discarding words containing
uppercase characters and short lines), and then breaking the words
up into tokens. Three token methods are supported:

- vowel    Divide text into runs of consecutive vowels / consonants.
- digram   Each token is 2 consecutive characters
- trigram  Each token is 3 consecutive characters

The trigram method generates the most realistic strings, but they will 
be long, because resulting entropy is low. Vowel method generates words that
are quite memorable, and have high entropy (only brute force will break
suck passwords; they are immunte to dictionary attack.
Digram method is somewhere in between.

By default, makelang assumes Project Gutenberg files,
meaning that anything before the words 'START OF THE PROJECT GUTENBERG ...'
is ignored. -no-guten can be specified if passing arbitrary text.

## Fewpass
This inspirational tool generates a few passphrases from each lang file.
~~~
$ ./fewpass
              lang/1.lang      whystheusunlydoeseigoo-60
              lang/1.lang          spophinkights-niof-25
              lang/1.lang           yeteeks-petharrio-36
          lang/alice.lang           eedn-eedn-cooreas-11
          lang/alice.lang        shirds-twirls-wrifth-18
          lang/alice.lang            thund-kemblysawl-17
        lang/beowulf.lang          oardords-outshostoo-8
        lang/beowulf.lang          diousluppeatnervidl-2
        lang/beowulf.lang            prarsh-tloo-rds-q-3
           lang/blah.lang   glus-flig-gleh-flot-flugi-20
           lang/blah.lang flut-gleset-fleset-flot-flu-33
           lang/blah.lang       flususegot-gluset-flu-78
     lang/donquijote.lang             ptardarnice-meng-2
     lang/donquijote.lang                rarencibi-pa-88
     lang/donquijote.lang              furictr-amatus-40
         lang/expect.lang         aidiouslyhongs-clie-25
         lang/expect.lang            zearchidiouslept-13
         lang/expect.lang             moofs-luencongh-60
     lang/federalist.lang          yo-prortheatrounds-41
     lang/federalist.lang    proaths-wrourths-yetuusie-7
     lang/federalist.lang            snallnersh-w-wisq-1
          lang/grimm.lang          cave-today-rein-abl-5
          lang/grimm.lang       rels-guisessed-fortab-81
          lang/grimm.lang      pickings-split-duty-nur-1
          lang/iliad.lang          yokes-fetch-siege-erm
          lang/iliad.lang    omnipotent-saw-inmost-som-1
          lang/iliad.lang  gigantagora-figural-lawful-18
       lang/mobydick.lang               ciasmayluenue-54
       lang/mobydick.lang          twinspearweastbaxl-17
       lang/mobydick.lang          schoo-amblandfalts-10
     lang/purereason.lang          evides-causr-guram-40
     lang/purereason.lang           kl-kritursit-tm-ns-1
     lang/purereason.lang           prop-ihenugeforen-81
    lang/shakespeare.lang              illyintaffreby-38
    lang/shakespeare.lang              oh-rovenagainue-1
    lang/shakespeare.lang              hydrablimerack-37
       lang/sherlock.lang          yearsortoochillnens-4
       lang/sherlock.lang             yetuadie-foie-k-36
       lang/sherlock.lang         smeartlydriedlerts-g-7
    lang/warandpeace.lang  voidable-unusuaded-viands-rus
    lang/warandpeace.lang      erupting-gems-snuffly-dim
    lang/warandpeace.lang     obdurally-elated-flicts-37
~~~

## Gutenpass
This is the main number->phrase converter. It has some interesting options
which I'll cover here.

- The -t or -numtail option allows replacing the last few characters
of the passphrase with a number. This does nothing to the password entropy
but may be required by some services.

~~~
    ./gutenpass -t 0
    quorecks-spraddyhaught

    ./gutenpass -t 10000
    a-ewahndt-jetz-4618
~~~

- The -nofreq option assumes that each symbol has frequency 1. This results in
shorter phrases at the expense of readability

~~~
    ./gutenpass 29465234946428   
    yer-aults-uptlydr-91
    ./gutenpass 29465234946428 -nofreq
    u-salonalarg-25
~~~

- The -maxsyllable option discards syllables longer than X. This results in shorter
phrases at the expense of flavor.

~~~
    ./gutenpass 29465234946428 -maxsyllable 1 -l lang/alice.lang 
    up-hucecalar-203
    /gutenpass 29465234946428 -maxsyllable 4 -l lang/alice.lang 
    youthouthushroor-ee-127
~~~

- The -l or -langfile option specifies which langfile to use. Default choice is to
pick a random one.

- The -nbits option controls the the approximate magnitude of the chosen random number.

~~~
    ./gutenpass -nbits 20 -t 0
    WARNING: Code 979570 is very small
    crysask
    ./gutenpass -nbits 60 -t 0
    cunninety-dimensified-royautre-vag
~~~

- The -minfreq X option discards syllables whose frequency is lower than X. This results
in very stereotypical but longer phrases.

~~~
    ./gutenpass -minfreq 500 -langfile lang/purereason.lang 
    ge-als-se-als-die-werden-ab-nich-is-105
    ./gutenpass -minfreq 1 -langfile lang/purereason.lang 
    gtensorgr-aberli-226
~~~

- The -n option generates several passphrases while all other parameters remain the same.
This is useful for choosing a good-sounding phrase from a list.

- The -i option reads numbers from stdin and produces passphrases for each number read

~~~
    (echo 765432345676543; echo 65434565432) | ./gutenpass -i -l lang/beowulf.lang 
    hlarmordlythouldegl-39
    you-whabba-795
~~~

- The -r option provides a useful report, including the actual entropy of the resulting
password. The actual entropy may be lower, especially with the trigram method, because the
generated word may be present in the English dictionary.
~~~
    ./gutenpass -r
    gutenpass  result:glet-flugogosut-glot-778  code:70973732737229  passbits:46  langfile:lang/blah.lang
~~~

- The -v (verbose) option gives debugging insights
~~~
    ./gutenpass -v
     code:7221805794614 prefix:- ntargets:72 sumfreq:143.160289401059 idx:59 freq:3 -> w
     code:151336781131 prefix:w ntargets:11 sumfreq:102.325192802057 idx:10 freq:35 -> i
     code:51764254672 prefix:i ntargets:173 sumfreq:232.126130051193 idx:57 freq:1 -> sb
     code:299215344 prefix:sb ntargets:3 sumfreq:100 idx:2 freq:60 -> u
     code:179529206 prefix:u ntargets:136 sumfreq:197.663108493604 idx:102 freq:1 -> lf
     code:1320067 prefix:lf ntargets:4 sumfreq:100.52380952381 idx:1 freq:1 -> u
     code:330016 prefix:u ntargets:136 sumfreq:197.663108493604 idx:30 freq:1 -> nm
     code:2426 prefix:nm ntargets:4 sumfreq:100 idx:1 freq:4 -> a

    wisbulfunma-606
~~~

## About Entropy
When I'm talking about the amount of entropy in a string, I'm referring
to the size of the smallest space from which the string can be recomputed.
For instance, if you call srand(time(0)) and then generate
a 128-character string, the resulting entropy would be only 25 bits or so, because the number
of possible strings would be equal to the number of seconds in a year.

Gutenpass by default generates passwords containing 48 bits of entropy
sampled from /dev/random. This may seem like a low number, but it is
practical: the resulting strings often have 70 bits of information and more, lang files
can be customized, and each gutenpass run has enough options to make
running it 2^48 times impractical. Its slowness is thus a security feature :-)

