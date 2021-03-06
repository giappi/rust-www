---
layout: sv-SE/default
title: Andra Installationssätt &middot; Programmeringsspråket Rust
---

# Andra Installationssätt för Rust

- [Vilken installation ska du använda?](#which)
- [Andra sätt att installera `rustup`](#more-rustup)
- [Fristående installationer](#standalone)
- [Källkod](#source)

## Vilka installation ska du använda?
<span id="which"></span>

Rust kan köras på många olika plattformar, och det finns många sätt att installera Rust på.
Det enklaste och rekommenderade sättet att installera Rust på hittar du [här][installation page].

Den här sidan beskriver installation via [`rustup`], ett verktyg som hanterar
flera Rust toolchains på ett konsekvent sätt på alla plattformar Rust stödjer.
Varför skulle man _inte_ vilja installera med hjälp av dessa instruktioner?

- Installation offline. `rustup` laddar ner komponenter från internet när det
  behövs. Om du behöver installera Rust utan tillgång till internet är `rustup`
  inte lämpligt.
- Preferens för (operativ)systemets pakethanterare. På Linux i synnerhet, men
  även på macOS med [Homebrew] och på Windows med [Chocolatey], föredrar
  utvecklare ibland att installera Rust med sin plattforms pakethanterare.
- Preferens emot `curl | sh`. På unix installerar vi vanligtvis `rustup` genom
  att köra ett shell-script via `curl`. Vissa oroas över säkerheten för detta
  arrangemang och föredrar att ladda ner och köra installationen själva.
- Verifierade signaturer. Även om `rustup` utför nedladdningar via HTTPS är det
  enda sättet att verifiera signaturerna för Rust-installationer idag att göra
  det manuellt med fristående installationer.
- Installation via grafiska gränssnitt och integrering med
  "Lägg till/ta bort program" på Windows. `rustup` kör i kommandotolken och
  registrerar inte sin installation som typiska Windows-program. Om du föredrar
  en mer typisk installation via grafiskt gränssnitt på Windows finns det
  självständiga `.msi` installationer. I framtiden kommer `rustup` även ha en
  installationer med grafiskt gränssnitt på Windows.

Rusts plattformsstöd definieras i [tre nivåer][three tiers] vilket nära
motsvarar de tillgängliga installationsmetoderna: i allmänhet tillhandahåller
Rust-projektet byggda binärer för alla plattformar i nivå 1 och nivå 2, och de kan
alla installeras via `rustup`.

Vissa nivå 2-plattformar har dock bara standardbiblioteket tillgängligt,
inte kompilatorn själv; det vill säga att de är endast mål för kross-kompilering.
Rust-kod kan köras på dessa plattformar, men de kan inte köra kompilatorn själv.
Sådana mål kan installeras med kommandot `rustup target add`.

## Andra sätt att installera `rustup`
<span id="rustup"></span>

Hur man installerar `rustup` skiljer sig mellan olika plattformar:

* På unix, kör `curl https://sh.rustup.rs -sSf | sh` i din terminal.
  Detta hämtar och kör [`rustup-init.sh`] som i sin tur hämtar och kör den
  korrekta versionen, för din plattform, av den körbara filen` rustup-init`.
* På Windows, ladda ner och kör [`rustup-init.exe`].

`rustup-init` kan konfigureras interaktivt, och alla alternativ kan dessutom
kontrolleras av kommandoradsargument som kan skickas genom shell-scriptet.
Skicka med `--help` till `rustup-init` för att visa argumenten som
`rustup-init` accepterar:

```
curl https://sh.rustup.rs -sSf | sh -s -- --help
```

Om du föredrar att inte använda shell-scriptet kan du direkt ladda ner
`rustup-init` för din plattform:

<div class="rustup-init-table">
  {% for column in site.data.platforms.rustup %}
  <div>
    {% for target in column %}
    {% if target contains 'windows' %}
    <a href="https://static.rust-lang.org/rustup/dist/{{ target }}/rustup-init.exe">
      {{ target }}
    </a>
    {% else %}
    <a href="https://static.rust-lang.org/rustup/dist/{{ target }}/rustup-init">
      {{ target }}
    </a>
    {% endif %}
    {% endfor %}
  </div>
  {% endfor %}
</div>

## Fristående installationer
<span id="standalone"></span>

De officiella fristående Rust installationerna innehåller en enda utgåva av Rust
och är lämpliga för offlineinstallationer. De finns i tre former: tarballs
(filändelse `.tar.gz`), som fungerar i alla unix-liknande miljöer,
Windows-installationer (` .msi`) och macOS-installationer (`.pkg`). Dessa
installationer levereras med: `rustc`,` cargo`, `rustdoc`, standardbiblioteket
och standarddokumentationen, men ger inte tillgång till ytterligare cross-targets
som `rustup` gör.

De vanligaste anledningarna till att använda dessa är:

- Offlineinstallation
- Du föredrar ett mer plattformsintegrerat grafiskt installationsprogram på Windows

Var och en av dessa binärer är signerad med
[Rusts signeringsnyckel][Rust signing key], som är
[tillgänglig på keybase.io][available on keybase.io],
av "Rust build infrastructure", med [GPG].
I tabellerna nedan är `.asc`-filerna signaturerna.

Tidigare utgåvor finns i [arkiv][the archives].

{% for channel in site.channels %}

### {{ channel.name | capitalize }} ({{ channel.vers }})
<span id="{{ channel.name }}"></span>

<div class="installer-table {{ channel.name }}">
  {% for column in site.data.platforms[channel.name] %}
  <div>
    {% for target in column %}
    <div>
      <span>{{ target }}</span>
      <a href="https://static.rust-lang.org/dist/rust-{{ channel.package }}-{{ target }}.tar.gz">.tar.gz</a>
      <a href="https://static.rust-lang.org/dist/rust-{{ channel.package }}-{{ target }}.tar.gz.asc">.asc</a>
    </div>
    {% if target contains 'windows' %}
    <div>
      <span>{{ target }}</span>
      <a href="https://static.rust-lang.org/dist/rust-{{ channel.package }}-{{ target }}.msi">.msi</a>
      <a href="https://static.rust-lang.org/dist/rust-{{ channel.package }}-{{ target }}.msi.asc">.asc</a>
    </div>
    {% elsif target contains 'darwin' %}
    <div>
      <span>{{ target }}</span>
      <a href="https://static.rust-lang.org/dist/rust-{{ channel.package }}-{{ target }}.pkg">.pkg</a>
      <a href="https://static.rust-lang.org/dist/rust-{{ channel.package }}-{{ target }}.pkg.asc">.asc</a>
    </div>
    {% endif %}
    {% endfor %}
  </div>
  {% endfor %}
</div>

{% endfor %}

## Källkod
<span id="source"></span>

<div class="installer-table">
  <div>
    <div>
      <span>Stabil</span>
      <a href="https://static.rust-lang.org/dist/rustc-{{ site.stable }}-src.tar.gz">.tar.gz</a>
      <a href="https://static.rust-lang.org/dist/rustc-{{ site.stable }}-src.tar.gz.asc">.asc</a>
    </div>
  </div>
  <div>
    <div>
      <span>Beta</span>
      <a href="https://static.rust-lang.org/dist/rustc-beta-src.tar.gz">.tar.gz</a>
      <a href="https://static.rust-lang.org/dist/rustc-beta-src.gz.asc">.asc</a>
    </div>
  </div>
  <div>
    <div>
      <span>Nattlig</span>
      <a href="https://static.rust-lang.org/dist/rustc-nightly-src.tar.gz">.tar.gz</a>
      <a href="https://static.rust-lang.org/dist/rustc-nightly-src.tar.gz.asc">.asc</a>
    </div>
  </div>
</div>

[installation page]: install.html
[`rustup`]: https://github.com/rust-lang-nursery/rustup.rs
[other-rustup]: https://github.com/rust-lang-nursery/rustup.rs#other-installation-methods
[`rustup-init.exe`]: https://static.rust-lang.org/rustup/dist/i686-pc-windows-gnu/rustup-init.exe
[`rustup-init.sh`]: https://static.rust-lang.org/rustup/rustup-init.sh
[Homebrew]: http://brew.sh/
[Chocolatey]: http://chocolatey.org/
[three tiers]: https://forge.rust-lang.org/platform-support.html
[Rust signing key]: https://static.rust-lang.org/rust-key.gpg.ascii
[GPG]: https://gnupg.org/
[available on keybase.io]: https://keybase.io/rust
[the archives]: https://static.rust-lang.org/dist/index.html
