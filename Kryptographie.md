<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kryptographie</title>
  <link rel="stylesheet" href="https://stackedit.io/style.css" />
</head>

<body class="stackedit">
  <div class="stackedit__left">
    <div class="stackedit__toc">
      
<ul>
<li><a href="#kryptographie">Kryptographie</a>
<ul>
<li><a href="#inhalte">Inhalte</a></li>
<li><a href="#literatur">Literatur</a></li>
<li><a href="#grundbegriffe">Grundbegriffe</a></li>
<li><a href="#klassische-symmetrische-verschlüsselungsverfahren">Klassische Symmetrische Verschlüsselungsverfahren</a></li>
<li><a href="#die-vigenère-chiffre">Die Vigenère-Chiffre</a></li>
</ul>
</li>
</ul>

    </div>
  </div>
  <div class="stackedit__right">
    <div class="stackedit__html">
      <h1 id="kryptographie">Kryptographie</h1>
<h2 id="inhalte">Inhalte</h2>
<ul>
<li>Überblick + Grundbegriffe</li>
<li>Klassische symmetrische Verschlüsselungssysteme</li>
<li>Mathe - Intermezzo
<ul>
<li>Teiler</li>
<li>Primzahlen</li>
<li>Kongruenzen</li>
<li>ggT berechnen entwickelten</li>
</ul>
</li>
<li>Moderne symmetrische Verfahren
<ul>
<li>DES (Mini-DES)</li>
<li>AES (Advanced Encryption Standard)
<ul>
<li>Exkurs über Galois Felder :)</li>
</ul>
</li>
</ul>
</li>
<li>Public-Key-Verschlüsselungssysteme
<ul>
<li>RSA - Algorithmus</li>
<li>Diffie - Hellman Key Exchange</li>
<li>ElGamal dig. Signatur</li>
<li>Elliptische Kurven Kryptographie</li>
</ul>
</li>
</ul>
<h2 id="literatur">Literatur</h2>
<ul>
<li>Simon Singh: Geheime Botschaften, Hanser-Verlag</li>
<li>D. Kippenhahn: Verschlüsselte Botschaften, Rowohld</li>
<li>David Kahn: The Codebreakers, Scribner NY 1997</li>
<li>Bruce Schneier: Applied Cryptography</li>
<li>Klaus Schunek: Kryptographie</li>
<li>Script</li>
</ul>
<h2 id="grundbegriffe">Grundbegriffe</h2>
<h3 id="was-ist-kryptologie">Was ist Kryptologie?</h3>
<p>Kryptologie ist eine Wissenschaft, die sich mit Methoden der Verschlüsselung und damit verwandte Verfahren befasst.</p>
<ul>
<li>Personen: Kryptologen -&gt; High-End Mathematiker</li>
<li>Kryptologie
<ul>
<li>Kryptographie</li>
<li>Kryptoanalysis</li>
</ul>
</li>
</ul>
<p>Kryptographie bezeichnet den Zweig der Kryptologie, der sich mit der Erstellung und dem Design kryptologischer Algorithmen befasst. In der Kryptoanalyse geht es um das nicht-autorisierte Brechen von Verschlüsselungen.</p>
<p>Der Kryptologie liegt ein einfaches Kommunikationsmodell zugrunde:</p>
<ul>
<li>Alice und Bob wollen vertraulich kommunizieren, das heißt eine Nachricht die Alice sendet, soll nur von Bob gelesen werden können (und umgekehrt).
<ul>
<li>Telefonverbindung</li>
<li>Email-Austausch</li>
<li>Internetverbindung</li>
<li>Speichervorgang mit (viel) späterem Lesezugriff.</li>
</ul>
</li>
<li>Ein Angreifer in diesem Modell heißt Eve (von eavesdropping = lauschen). Ziel ist es, die Kommunikation (passiv oder aktiv) zu beeinflussen.</li>
</ul>
<p>Bei der Kryptographie geht es um folgendene Punkte:</p>
<ul>
<li><strong>Geheimhaltung</strong>. Ein kryptographisches System, das Geheimhaltung gewährleistet, verhindert die Entnahme von Informationen durch eine dritte, nicht autorisierte Partei währen der Übertragung über eien unsicheren Kommunikationsverlauf. Der Sender einer Nachricht kann sicher sein, dass nur der legitime Empfänger die Nachricht lesen kann.</li>
<li><strong>Authentizität</strong>. Ein kryptografisches System, das Authentizität gewährleistet, verhindert das nicht-autorisierte Einschleusen einer Nachricht in einen unsicheren Kommunikationskanal. Der Empfänger kann sicher sein, dass die Nachricht vom angegebenen Sender stammt.</li>
<li><strong>Digitale Signatur</strong>. Der Empfänger einer Nachricht kann eine neutrale Partei davon überzeugen, wer die Nachricht verfasst hat.</li>
<li><strong>Integrität</strong>. Ein Kryptografisches System, aus Integrität gewährleistet, verhindert die nicht-autorisierte Manipulation des Inhalts einer Nachricht, während der ÜBertragung, über einen unsicheren Kanal.</li>
<li><strong>Verbindlichkeit</strong>. Sender und Empfänger soll es nicht möglich sein, im Nachhinein zu behaupten, die Nachricht nicht gesendet/empfangen zu haben.</li>
</ul>
<p>Weiter Begriffe:</p>
<ul>
<li>Identifikation bedeutet die Zuordnung einer Identität zu einem Subjekt z.B. Benutzername.</li>
<li>Authentifizierung. Verifikation der Gültigkeit einer Tatsache -&gt; Passwort/Primzahlen</li>
<li>Datenschutz. Hier geht es um den Umgang, mit personenbezogenen Daten. Hier geht es um die Frage, ob personenbezogene Daten überhaupt erhoben werden dürfen, und/oder verarbeitet werden/anonymisiert.</li>
<li>Datensicherheit. Hier geht es um den Schutz von Daten (personenbezogen oder nicht)</li>
</ul>
<p><strong>Definition</strong>: Der Klartext ist die Nachricht, die durch ein geeinetes Verfahren in eine Form transformiert wird, dass eine Informationsentanhme durch eine 3. Partei nicht möglich ist.</p>
<p>Klartext zu…</p>
<ul>
<li>natürliche Sprachen</li>
<li>Morse Zeichen</li>
<li>Bitfolgen</li>
</ul>
<p>Es gibt zwei grundlegende Verfahren, eine Nachricht vor Informationsentanhme zu schützen:</p>
<ul>
<li>Kryptographische Verfahren (“unlesbar machen”)</li>
<li>Steganographie. Hier geht es um ein Vefahren, die eigentliche Existenz (nicht dem Inhalt) einer Nachricht verstecken. Heute digitale Wasserzeichen.</li>
</ul>
<p>Das Verfahren, einen Klartext in eine Form zu transformieren, dass die Informationsentnahme nicht möglich ist, heisst Verschllüsselung, oder Chiffrieren, eine verschlüsselte Nachricht heißt Chiffretext. Codieren, hat eine andere Bedeutung, nämlich Umwandlung in eine andere Form, damit sie geeignet verarbeitet werden können.</p>
<p>Die Umkehrung heißt Entschlüsselung oder Dechffrierung. Ein Kryptographischer Algorithmus (oder auch Chiffre) ist das mathematische Verfahren, welches den Klartext im Chiffretext umwandelt. Üblicherweise verwenden heutige krypto. Verfahren einen Schlüssel K. Das ist ein bestimmer Wert aus einem Schlüsselraum.<br>
z.B. K = 128Bit Schlüsselraum</p>
<p>Daumenregel: 10^3 = 1024 ~= 10^3</p>
<h3 id="grundlegenes-szenario-symmetrische-verschlüsselung">Grundlegenes Szenario (Symmetrische Verschlüsselung)</h3>
<ul>
<li>Klartext(@Alice) —&gt; <strong>Verschl. Algorith.(K)</strong> —Chiffretext(unsicherer Kommunikationskanal)–&gt; <strong>Entschl. Alg.(K)</strong> —&gt; Klartext(@Bob)</li>
<li>Sender und Empfänger benötigen den gleichen Schlüssel <strong>K</strong> zum Ver-/Entschlüsseln (daher symmetrische Verschlüsselung)</li>
</ul>
<p>Symmetrische Verfahren haben (alle) drei grundsätzliche Probleme</p>
<ol>
<li>Key Exchange Probleme</li>
</ol>
<ul>
<li>Der Schlüssel <strong>K</strong> muss auf sicherem Weg von Alice zu Bob gelangen.</li>
</ul>
<ol start="2">
<li>Key Management Problem</li>
</ol>
<ul>
<li>Bei n Parteien (n*(n-1))/2 Schlüssel benötigt</li>
</ul>
<ol start="3">
<li>Mit symmetrischen Verfahren ist es nicht möglich, digitale Signatur zu realisieren. Es gibt kein eindeutiges Indentifikationsmerkmal für den Sender einer Nachricht.</li>
</ol>
<h3 id="grundlegenes-szenario-asymmetrische-verschlüsselung">Grundlegenes Szenario (Asymmetrische Verschlüsselung)</h3>
<p>1976 Whitfield Diffie + Martin Hellman: “New Directions in Cryptography” =&gt; Public Key Kryptographie oder Asymmetrische Verschlüsselung</p>
<ul>
<li>Szenario 1: Alice will Bob eine vertrauliche Nachricht zukommen lassen (ohne Key-Exchange)
<ol>
<li>Bob erzeugt zwei Schlüssel, <strong>K_priv</strong>, <strong>K_publ</strong>. <strong>K_priv</strong> ist nur Bob bekannt. <strong>K_pub</strong> kann veröffentlich werden.</li>
<li>Alice besorgt sich <strong>K_pub</strong> von Bob</li>
<li>Alice verschlüsselt den Klartext mib Bobs <strong>K_pub</strong> -&gt; Chiffretext</li>
<li>Chiffretext geht an Bob</li>
</ol>
<ul>
<li>(Bedingung: D_K_pub (E_K_pub(M)) != M)</li>
</ul>
<ol start="5">
<li>Bob entschlüsselt das Chiffrat mit seinem <strong>K_priv</strong></li>
</ol>
<ul>
<li>(Bedingung D_K_priv [ E_K_pub(M)] == M)</li>
</ul>
</li>
</ul>
<p>Key Exchange-Problem gelöst, da Alice und Bob keinen Schlüssel über einen sicheren Kanal austauschen müssen!</p>
<p>Key Management-Problem gelöst, da ein Schlüssel ausreicht (K_pub), so dass beliebig viele Parteien vertraulich mit Bob kommunizieren können.</p>
<p>In der Praxis: Public-Key-Verfahren sind etwa 1000x langsamer als symmetrische Verfahren -&gt; man verwendet daher sog. <strong>Hybridverfahren</strong></p>
<ul>
<li>
<p>D.h. Public-Key Verfahren werden genutzt, um zwischen Sender und Empfänger einen “Sitzungsschlüssel” auszutauschen. Die Nutzdaten werden dann symmetrisch verschlüsselt, da Sender + Empfänger einen gemeinsamen Schlüssel besitzen.</p>
</li>
<li>
<p>Szenario 2: Alice will Bob ein signiertes Dokument zukommen lassen.</p>
<ol>
<li>Alice erzeugt ein Schlüsselpaar (<strong>K_priv</strong>, <strong>K_pub</strong>)</li>
<li>Alice “verschlüsselt” den Klartext mit ihrem <strong>K_priv</strong> (den nur Alice hat). -&gt; Chiffrat</li>
</ol>
<ul>
<li>Idee: D_K_pub [E_K_priv(M)] = M</li>
</ul>
<ol start="3">
<li>Chiffrat geht an Bob</li>
<li>Bob besorgt sich <strong>K_pub</strong> und dechiffriert das erhaltene Chiffrat</li>
<li>Entsteht Klartext, dann ist auch für eine neutrale 3. Partei gezeigt, das ALice die Nachricht verfasst hat. In der Praxis geht’s etwas anders:</li>
</ol>
<ul>
<li>Klartext --&gt; Hash Alg. --&gt; Hash Wert</li>
<li>Das Haswort garantiert die Integrität, d.h. das ist das Instrument um zu prüfen, ob der KLartext während der ÜBertragung manipuliert wurde.</li>
</ul>
<ol start="6">
<li>Bob erhält Klartext und Signierter Hash Wert</li>
</ol>
<ul>
<li>Klartext -&gt; Hash-Alg -&gt; Hash Wert</li>
<li>Sign Hw Entschlüsseln</li>
<li>Wenn Hashwerte übereinstimmen alles ok.</li>
</ul>
</li>
</ul>
<p>Es gibt Dutzende symmetrische Verfahren, z.B.</p>
<ul>
<li>AES = Advanced Encryption Standard (von Joan Daemon und Vince Djiman) zertifiziert von NIST</li>
<li>DES = Vorgänger von AES, stammt von IBM</li>
<li>Blowfish/Twofish von Bruce Schneier</li>
<li>IDEA</li>
<li>Lucifer</li>
<li>…</li>
</ul>
<p>Public-Key-Verfahren:</p>
<ul>
<li>RSA = Rivest, Shamir, Adleman</li>
<li>DH-Key-Exchange (Diffie-Hellmann)</li>
<li>ElGamal-Verfahren (Signatur)</li>
<li>Elliptische Kurven Kryptographie</li>
</ul>
<p>Sowohl symmetrische als auch asymmetrische Verfahren können in Form von <strong>Block-Chiffren</strong> oder <strong>Strom-Chiffren</strong> eningesetzt werden. Bei Blockchiffren werden Bitblöcke (z.B. 64 Bit) in einem Vorgang verschlüsselt in einem 64 Bit Chiffreblock. Klartext -&gt; aufteilen in n 64 Bit Blöcken. Bei Stromchiffren (bzw. bei WLAN) wird Zeichen für Zeichen verschlüsselt (bitweise/byteweise).</p>
<p>Es gibt in den meisten Kryptografischen Systemen zwei grundlegende Operationen wie Klartext in Chiffretext transformiert wird.</p>
<ul>
<li>Substitution: Bei einer Substitutionsoperation wird ein Klartextzeichen (oder ein Klartextblock) durch ein anderes Zeichen ersetzt. Die Position des Zeichens/Blocks bleibt erhalten.</li>
<li>Transposition (Permutation, Vertauschung): Transpositionsoperationen vertauschen die Reihenfolge der Zeichen, d.h. die Position der Klartextzeichen unterhalb des Texts. Die Form bleibt erhalten.</li>
</ul>
<p>Moderne Verfahren benutzen eine Vielzahl von Substitutions- bzw. Transpositiosoperationen.</p>
<p><strong>Definition</strong>: (Claude Shannan 1948): Ein Kryptosystem ist ein Fünf-Tupel <strong>K = (P,C,K,E,D)</strong>.</p>
<ul>
<li>P: Endliche Menge, Klartext Alphabet</li>
<li>C: Endliche Menge, Chiffretextalphabet</li>
<li>K: Endliche Menge, Schlüsselraum</li>
<li>E: Zu jedem Wert k e K (zu jedem Schlüssel k), gibt es einen Verschlüsselungalgorithmus ek e E und einem zugeordneten Entschlüsselungsalgorithmus.</li>
</ul>
<p>Also: Das Design eines Kryptosystems erforder die Spezifikation von fünf Größen</p>
<ol>
<li>Klartextalphabet</li>
<li>Chiffretextalphabet</li>
<li>Schlüsselraum</li>
<li>Verschlüsselungsverfahren</li>
<li>Entschlüsselungsverfahren</li>
</ol>
<p>In der heutigen Kryptographie wird in der Regel streng auf das <strong>KERCKHOFFS-Prinzip</strong> geachtet (1883). “Die Sicherheit eines Kryptosystems darf niemals von der Geheimhaltung des Verschlüsselungsalgorithmus abhängen, sondern ausschließlich von dem Geheimhalten des Schlüssels.”</p>
<h3 id="kurz-who-is-who-in-der-kryptografie-keinesfalls-vollständig">kurz: Who is who in der Kryptografie (keinesfalls vollständig)</h3>
<ul>
<li>ANSI (American National Standars Institute)
<ul>
<li>ASC X9 (Arbeitsgruppe), Empfehlungen für sichere Finanztreuaktionen</li>
</ul>
</li>
<li>NSA</li>
<li>GCHQ Government Communication Headquaters</li>
<li>NIST National Institute of Standards and Technology
<ul>
<li>FIPS</li>
</ul>
</li>
<li>iSO International Organisation for Standardization</li>
<li>IEEE</li>
<li>NESSIE-Projekt New European Schemes for Signatures, Integrity and Encryption</li>
<li>Bruce Schneier</li>
<li>Ron Rivest MIT</li>
<li>BSI Bundesamt für Sicherheit in der Informationstechnologie</li>
<li>Phil Zimmermann -&gt; PGP = Pretty Good Privacy</li>
</ul>
<h2 id="klassische-symmetrische-verschlüsselungsverfahren">Klassische Symmetrische Verschlüsselungsverfahren</h2>
<ol>
<li>Klartext wird in Kleinbuchstaben geschrieben</li>
<li>CHIFFRETEXT WIRD IN GROßBUCHSTABEN geschrieben</li>
<li>Im Klartext werden Leerzeichen und Interpunktionszeichen weggelassen</li>
<li>Die Klartext- und Chiffretextzeichen werden durch die Zahlen von 0 bis 25 dargestellt</li>
</ol>
<h3 id="die-shift-chiffre-oder-cäsar-chiffre">Die Shift-Chiffre (oder Cäsar-Chiffre)</h3>
<p>Das Shift-Chiffre ist eine sog. monoalphabetische Chiffre, d.h. das Klartextalphabet wird genau auf ein Chiffretextalphabet abgebildet. Dies hat zur Folge, dass die statistische Struktur des Klartexts erhalten bleibt.</p>
<p>Andere Frage: Hilft es, wenn man ein Klartext 2x hintereinander und 2 verschiedenen Schlüsseln verschlüsselt, also, erhöht sich dadurch die Sicherheit? Hier nicht, da das Hintereinanderausführen von zwei Shift-Chiffren mit den Schlüsseln K_1,K_2 wie einer Verschlüsselung mit K_1+K_2.</p>
<h3 id="die-affine-chiffre">Die Affine Chiffre</h3>
<p>Beispiel, KLartext: Wirtschaftsinformatik<br>
[<br>
(\alpha, \beta) = (5, 12)<br>
]<br>
[<br>
ggT(5,26) = 1<br>
]<br>
[<br>
w \mapsto 22 \mapsto 122</p>
<p>]</p>
<hr>
<p>Vorlesung 2</p>
<h2 id="die-vigenère-chiffre">Die Vigenère-Chiffre</h2>
<p>Die Shift- und die affine Chiffre sind Beispiele von monoalphabetischen Chiffren, d.h. mit der Wahl eines Schlüssels wird jedes Klartextzeichen auf genau ein Chiffretextzeichen abgebildet (Eigenschaft, dass die statistische Eigenschaft des Klartexts auf dem Chiffretext übertragen wird)</p>

    </div>
  </div>
</body>

</html>
