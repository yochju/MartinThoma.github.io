---
layout: post
title: Informationsfusion
slug: informationsfusion
author: Martin Thoma
date: 2016-05-23 20:00
category: German posts
tags: Klausur
featured_image: logos/klausur.png
---
<div class="info">Dieser Artikel beschäftigt sich mit der Vorlesung &bdquo;Informationsfusion&ldquo; am KIT. Er dient als Prüfungsvorbereitung. Ich habe die Vorlesungen nicht gehört, aber die Folien von <a href="http://ies.anthropomatik.kit.edu/mitarbeiter.php?person=heizmann">Herrn Prof. Dr.-Ing. Michael Heizmann</a> aus dem Wintersemester 2015/2016 gelesen.</div>

In der Vorlesung 'Informationsfusion' ist der Kalman-Filter ein zentraler
Inhalt.

## Behandelter Stoff

### Grundlagen

Slides: `IF-Kap1_151110.pdf`

Es wurden Grundbegriffe wie Daten, Information, Merkmal, Informationsfusion,
Signal, usw. eingeführt.

<dl>
    <dt><dfn id="information">Information</dfn></dt>
    <dd>

        Information ist alles was potentiell zur Verringerung von Ungewissheit
        beiträgt.

        Sinnvolle Informationen besteht aus Fakten und zugehörigen
        Unsicherheiten.

    </dd>
    <dt><dfn id="signal">Signal</dfn></dt>
    <dd>Ein Signal ist eine Funktion oder Wertefolge welche Information trägt.</dd>
    <dt><dfn id="daten">Daten</dfn></dt>
    <dd>Daten sind maschinenlesbare Repräsentationen von Informationen. Sie
        werden als Zeichen oder Zeichenketten gespeichert.</dd>
    <dt><dfn id="feature-dfn">Merkmal</dfn></dt>
    <dd>Ein Merkmal ist eine beobachtbare oder physikalisch messbare Eigenschaft eines oder mehrerer Objekte.</dd>
    <dt><dfn>Vorraussetzungen für Informationsfusion</dfn></dt>
    <dd>
        
        <ul>
            <li>Gemeinsamer Sachverhalt
            <ul>
                <li>Kompatibler Definitionsbereich</li>
                <li>Kompatibler Wertebereich</li>
            </ul>
            </li>
            <li>Unsicherheiten: Die Informationen müssen ein Maß für ihre Unsicherheit tragen</li>
        </ul>

    </dd>
    <dt><dfn id="informationsfusion-advantages">Vorteile von Informationsfusion</dfn></dt>
    <dd>

        <ul>
            <li>Höhere Robustheit</li>
            <li>Erweterung der Sensorabdeckung</li>
            <li>Erhöhte Auflösung (z.B. Accelerometer + Kompas in Kamera)</li>
            <li>Kostenreduktion (z.B. mehrere billige Bildsensoren, dann Daten mitteln zur Rauschreduktion)</li>
            <li>Unsicherheit Verringern (z.B. FLIR + Radar)</li>
            <li>Indirektes schließen auf Größen (z.B. Oberflächennormalen)</li>
        </ul>
    </dd>
    <dt><dfn id="features-properties">Wünschenswerte Eigenschaften von Merkmalen</dfn></dt>
    <dd>

        <ul>
            <li>Leicht gewinnbar</li>
            <li>Interpretierbar</li>
            <li>Hohe Relevanz: Merkmalsvektor ist gut für die Aufgabe geeignet (z.B. lineare separierbarkeit der Klassen bei Klassifikationsproblemen in Merkmalsraum)</li>
            <li>Robustheit gegen Störungen</li>
            <li>Invarianzen werden berücksichtigt (z.B. Drehung des Objekts)</li>
            <li>Geringe Dimensionalität des Merkmalsvektors</li>
            <li>Geringe Abhängigkeit zwischen Merkmalen</li>
        </ul>

    </dd>
    <dt><dfn id="sensorsystem-properties">Eigenschaften von Sensorsystemen</dfn></dt>
    <dd>

        <ul>
            <li>Homogenität / Heterogenität</li>
            <li>Wirkmechanismus / Struktur</li>
            <li>Zuverlässigkeit</li>
            <li>Kommensurabilität (Gleichdimensionalität)</li>
            <li>Kollokiertheit (Identische Ausschnitte der Szene): Falls nicht gegeben, ist Registrierung erforderlich</li>
        </ul>

        Weitere:

        <ul>
            <li>Aktive / passive Sensoren: z.B. Laser beeinflusst die Umwelt</li>
            <li>Virtuelle Sensorsysteme: Gleiche Sensoren, aber unterschiedliche Parameter</li>
            <li>Kosten, Material</li>
        </ul>

    </dd>
</dl>


### WT

Slides: `IF-Kap2_151215.pdf`

* Wahrscheinlichkeitsraum, Zufallsvariable
* Guide to the Expression of Uncertainty in Measurement (GUM)
* Bayessche Methodik

<dl>
    <dt><a href="https://en.wikipedia.org/wiki/Probability_axioms#Axioms"><dfn id="kolmogorov-axioms">Kolmogorov-Axiome</dfn></a></dt>
    <dd>Siehe <a href="https://martin-thoma.com/probabilistische-planung#probability-measure">Probabilistische Planung</a></dd>
    <dt><a href="https://de.wikipedia.org/wiki/Kovarianz_(Stochastik)"><dfn>Kovarianz</dfn></a> (<dfn id="covariance">Covariance</dfn>)</dt>
    <dd>Es seien $X, Y$ Zufallsvariablen. Dann heißt
        $$COV(X, Y) = \mathbb{E}((X - \mathbb{E}(X)) \cdot (Y - \mathbb{E}(Y)))$$
        die Kovarianz von $X$ und $Y$.
    </dd>
    <dt><dfn id="statistisches-modell">Statistisches Modell</dfn></dt>
    <dd>Es sei $X$ eine Zufallsvariable. Dann heißt ein Tupel
        $(X, (P_\theta)_{\theta \in \Theta})$
        ein statistisches Modell, wenn $(P_\theta)_{\theta \in \Theta}$ eine
        Familie von Wahrscheinlichkeitsverteilungen ist.
    </dd>
    <dt><a href="https://de.wikipedia.org/wiki/Sch%C3%A4tzfunktion"><dfn id="schaetzer">Schätzer</dfn></a></dt>
    <dd>
        Sei $\mathcal{X}_n = (X_1, \dots, X_n)$ eine Stichprobe und
        $\theta$ ein Parameter. Dann heißt die Abbildung
        $$T: \mathcal{X} \rightarrow \tilde{\Theta}$$
        mit $\tilde{\Theta} \supseteq \Theta$ ein Schätzer für $\theta$.
    </dd>
    <dt><dfn>Konsistenter Schätzer</dfn></dt>
    <dd>

        Sei $\mathcal{X}_n = (X_1, \dots, X_n)$ eine Stichprobe, $\theta$ ein Parameter und
        $T(\mathcal{X}_n)$ ein Schätzer für $\theta$. $T(\mathcal{X}_n)$
        heißt konsistent, wenn gilt:

        $$\lim_{n \rightarrow \infty} P_\theta (|T(\mathcal{X}_n) - \theta| \geq \varepsilon) = 0$$

    </dd>
    <dt><dfn>Asymptotisch Erwartungstreuer Schätzer</dfn></dt>
    <dd>

        Ein Schätzer $\hat{\theta} = \hat{\theta}(X_1, \dots, X_n)$ heißt
        asymptotisch erwartungstreu, wenn der Grenzwert der zu schätzenden
        Folge unter Annahme von $\theta$ gleich $\theta$ ist:

        $$\lim_{n \rightarrow \infty} \mathbb{E}(\hat{\theta}) = \theta$$

    </dd>
    <dt><a href="https://de.wikipedia.org/wiki/Kalman-Filter"><dfn id="kalman-filter">Kalman-Filter</dfn></a> (<dfn>KF</dfn>)</dt>
    <dd>Siehe <a href="https://martin-thoma.com/kalman-filter/">Kalman-filter Artikel</a>.</dd>
    <dt><dfn id="extended-kalman-filter">Extended Kalman Filter</dfn> (<dfn id="ekf">EKF</dfn>)</dt>
    <dd>Siehe <a href="https://martin-thoma.com/kalman-filter/">Kalman-filter Artikel</a>.</dd
    <dt><a href="https://de.wikipedia.org/wiki/GUM_(Norm)"><dfn id="gum">GUM</dfn></a> (<dfn>Guide to the Expression of Uncertainty in Measurement</dfn>)</dt>
    <dd>GUM ist eine internationale Norm welche das Ziel hat, die
        Vergleichbarkeit zwischen Messergebnissen herzustellen. Dazu
        wurden in der Norm Grundsätze und Vorgehensweisen zur Bestimmung der
        Messunsicherheit festgelegt.<br/>
        <br/>
        GUM ist auf metrische Merkmale beschränkt.<br/>
        Vorgehen:
        <ol>
            <li>Modellgleichung formulieren: $Y = f(X_1, \dots, X_n)$, wobei
                $Y$ die Messgröße und $X_i$ die Eingangsgrößen sind.</li>
            <li>Eingangsgrößen und Unsicherheiten bestimmen (entweder durch Messreihen oder durch Erfahrungswerte / Handbücher)</li>
            <li>Schätzwert $\hat{y}$ für Messgröße $Y$ bestimmen</li>
            <li>Ermittlung der kombinierten Unsicherheit</li>
        </ol>
        </dd>
    <dt><dfn>Standardunsicherheit</dfn></dt>
    <dd>Die Standardunsicherheit einer Messung ist

        $$u_i = s(\bar{x_i}) = \sqrt{\frac{s^2(x_i)}{n}}$$</dd>
</dl>


### Dempster-Shafer-Theorie

Slides: `IF-Kap3_160125.pdf`

For this chapter, I highly recommend reading [Anwendung der Dempster-Shafer Evidenztheorie auf die Bonitätsprüfung](https://statistik.econ.kit.edu/download/Artikel%20-%20Anwendung%20der%20Dempster-Shafer%20Evidenztheorie%20auf%20die%20Bonit%C3%A4tspr%C3%BCfung.pdf).

<dl>
    <dt><dfn id="frame-of-discernment">Frame of discernment</dfn> (<dfn>Wahrnehmungsrahmen</dfn>)</dt>
    <dd>

        Der Wahrnehmungsrahmen ist eine Menge $\Omega$. Die Elemente dieser
        Mengen heißen Alternativen oder Aussagen. Eine Hypothese ist eine
        Teilmenge $H \subseteq \Omega$ des Wahrnehmungsrahmens.

    </dd>
    <dt><dfn>Basismaß</dfn> (<dfn id="basic-probability-mass">basic probability mass</dfn>)</dt>
    <dd>

        Sei $\Omega$ ein Wahrnehmungsrahmen und

        $$m: \mathcal{P}(\Omega) \rightarrow [0, 1]$$

        eine Abbildung von der Potenzmenge von $\Omega$ in das
        Einheitsintervall. $m$ heißt <i>Basismaß</i>, wenn gilt:

        <ul>
            <li>$m(\emptyset) = 0$</li>
            <li>$\sum_{X \subseteq \Omega} m(X) = 1$</li>
        </ul>

    </dd>
    <dt><dfn id="belief-function">Belief function</dfn> (<dfn>Glaubensfunktion</dfn>)</dt>
    <dd>

        Sei $\Omega$ ein Wahrnehmungsrahmen, $m$ ein Basismaß und
        $$Bel: \mathcal{P}(\Omega) \rightarrow [0, 1]$$
        eine Funktion. $Bel$ heißt Glaubensfunktion, wenn gilt:

        $$Bel(X) := \sum_{Y \subseteq X} m(Y)$$

        Die Glaubensfunktion stellt also eine untere Grenze für eine unbekannte
        Wahrscheinlichkeitsfunktion dar.

    </dd>
    <dt><dfn id="plausibility-function">Plausibility function</dfn> (<dfn>Plausibilitätsfunktion</dfn>)</dt>
    <dd>

        Sei $\Omega$ ein Wahrnehmungsrahmen, $m$ ein Basismaß und
        $$Pl: \mathcal{P}(\Omega) \rightarrow [0, 1]$$
        eine Funktion. $Pl$ heißt Plausibilitätsfunktion, wenn gilt:

        $$Pl(X) := \sum_{Y \cap X \neq \emptyset} m(Y)$$

        Die Plausibilitätsfunktion stellt also eine obere Grenze für eine unbekannte
        Wahrscheinlichkeitsfunktion dar.

    </dd>
    <dt><dfn>Fokale Ereignisse</dfn></dt>
    <dd>

        Ein Ereignis $A$ heißt fokal bzg. eines Basismaßes $m$, wenn $m(A) \neq 0$ gilt.

    </dd>
    <dt><dfn>Dempsters Kombinationsregel</dfn> (<dfn>Dempsters rule of combination</dfn>, <dfn id="drc">DRC</dfn>)</dt>
    <dd>$$m_1 \oplus m_2 (A) := \begin{cases}0&\text{for } A = \emptyset\\
                                             \frac{\sum_{X, Y: X \cap Y = A} m_1(X) m_2(Y)}{|1-K|}\end{cases}$$
        für Konfliktgrad $$K := \sum_{X, Y: X \cap Y = \emptyset} m_1(X) m_2(Y)$$
        Bei einem Konfliktgrad von $0 < K < 1$ spricht man von einem
        partiellen Konflikt. Ist der Konfliktgrad gleich $K=1$, so ist DRC
        nicht anwendbar.<br/>
        <br/>
        DRC ist assoziativ und kommutativ, allerdings nicht idempotent.
        Es gilt also im Allgemeinen nicht $m \oplus m = m$.<br/>
        Bei der Berechnung des Konfliktgrades genügt es fokale Ereignisse zu
        betrachten.</dd>
    <dt><dfn id="bayes-fusion">Bayessche Fusion</dfn></dt>
    <dd>

        Angenommen man hat eine Klassifikationsaufgabe. $z$ gehört einer der
        Klassen $A, B, C$ an. Nun liefert ein Klassifizierer $d_1$ die
        Wahrscheinlichkeitsverteilung

        $$m_1(A) = 0.01 \qquad m_1(B) = 0.99 \qquad m_1(C) = 0$$

        und ein zweiter Klassifizierer $d_2$ liefert

        $$m_2(A) = 0.01 \qquad m_2(B) = 0 \qquad m_2(C) = 0.99$$

        Gesucht ist eine Wahrscheinlichkeitsverteilung, welche die beiden
        Ergebnisse fusioniert.<br/>
        <br/>

        Da kein Vorwissen existiert, wird das Maximum-Entropie-Prinzip für die
        a priori Wahrscheinlichkeitsverteilung verwendet. Man geht also a
        priori davon aus, dass jede Klasse gleich wahrscheinlich ist:
        $$P(z) = (\frac{1}{3}; \frac{1}{3}; \frac{1}{3})$$<br/>
        <br/>

        Nun gilt:

$$
\begin{align}
    P(z | d_1, d_2)&= \frac{P(d_1, d_2 | z) \cdot P(z)}{P(d_1, d_2)}\\
                   &= \frac{P(d_1 | z) \cdot P(d_2 | d_1, z) \cdot P(z)}{P(d_1, d_2)}\\
                   &\overset{(1)}{=} \frac{P(d_1 | z) \cdot P(d_2 | z) \cdot P(z)}{P(d_1, d_2)}\\
                   &= \frac{\begin{pmatrix}0.1\\0.99\\0\end{pmatrix} \cdot \begin{pmatrix}0.1\\0\\0.99\end{pmatrix} \cdot \begin{pmatrix}1/3\\1/3\\1/3\end{pmatrix}}{P(d_1, d_2)}\\
                   &= \frac{\begin{pmatrix}1/300\\0\\0\end{pmatrix}}{P(d_1, d_2)}\\
                   &\overset{(2)}{=} \begin{pmatrix}1\\0\\0\end{pmatrix}\\
\end{align}
$$

        bei (1) wurde Unabhängigkeit vorausgesetzt, bei (2) wurde auf 1
        normiert, damit eine Wahrscheinlichkeitsverteilung herauskommt.

    </dd>
</dl>


### Fuzzy-Systeme

Slides: `IF-Kap4_160125.pdf`

Zur Einführung:

* [Fuzzy Logic - Computerphile](https://www.youtube.com/watch?v=r804UF8Ia4c)
* [Fuzzy Logic: An Introduction](https://www.youtube.com/watch?v=P8wY6mi1vV8)
* [An Introduction to Fuzzy Logic](https://www.youtube.com/watch?v=rln_kZbYaWc): An example with breaks

<dl>
    <dt><dfn>Zugehörigkeitsfunktion</dfn> (<dfn id="membership-function">membership function</dfn>)</dt>
    <dd>Sei $\Omega$ ein Grundraum und $A$ eine unscharfe Menge, für die
        $\mu_A: X \rightarrow [0, 1]$ den Grad der Zugehörigkeit definiert.

        $\mu_A$ heißt Zugehörigkeitsfunktion, wenn gilt
        $$\mu_{\Omega \setminus A}(t) = 1 - \mu_{A}$$</dd>
    <dt><a href="https://de.wikipedia.org/wiki/Satz_vom_ausgeschlossenen_Dritten"><dfn>Gesetz vom ausgeschlossenen Dritten</dfn></a></dt>
    <dd>Für eine beliebige Aussage muss mindestens die Aussage selbst oder ihr Gegenteil gelten. Dies gilt in der klassischen Mengenlehre, jedoch nicht für unscharfe Mengen.</dd>
    <dt><a href="https://de.wikipedia.org/wiki/Satz_vom_Widerspruch"><dfn>Gesetz vom ausgeschlossenen Widerspruch</dfn></a></dt>
    <dd>

        Zwei einander widersprechende Aussagen können nicht zugleich zutreffen.

        Dies gilt in der klassischen Mengenlehre, jedoch nicht für unscharfe Mengen.

    </dd>
    <dt><dfn>Fuzzy-Operationen</dfn></dt>
    <dd>

        Es seien $\mu_A, \mu_B$ die Zugehörigkeitsfunktionen zweier unscharfer
        Mengen $A, B$ über dem Grundraum $\Omega$. Dann gilt:<br/>
        <br/>
        Not:
        $$\forall x \in \Omega: \mu_{\Omega \setminus A}(x) = 1 - \mu_A(x)$$
        <br/>
        Konjunktion (AND, Minimum-T-Norm):
        $$\forall x \in \Omega: \mu_{A \land B}(x) = \min(\mu_A(x), \mu_B(x))$$
        <br/>
        Disjunktion (OR, Maximum-T-Norm):
        $$\forall x \in \Omega: \mu_{A \lor B}(x) = \max(\mu_A(x), \mu_B(x))$$

    </dd>
    <dt><dfn>Defuzzifizierung</dfn> (<a href="https://en.wikipedia.org/wiki/Defuzzification"><dfn>Defuzzification</dfn></a>)</dt>
    <dd>Unter Defuzzifizierung versteht man die Berechnung des scharfen
        Wertes der Ausgangsgröße.<br/>
        <br/>
        Methoden:

        <ul>
            <li><a href="#center-of-gravity-defuzzification">Schwerpunktverfahren</a></li>
            <li><a href="#mean-of-maxima-defuzzification">Maximum-Mittelwert-Methode</a></li>
        </ul>

    </dd>
    <dt><dfn>Schwerpunktverfahren</dfn> (<dfn id="center-of-gravity-defuzzification">center of gravity</dfn>, <dfn>COG</dfn>)</dt>
    <dd>Das Schwerpunktverfahren dient zur Defuzzifizierung.</dd>
    <dt><dfn>Maximum-Mittelwert-Methode</dfn> (<dfn id="mean-of-maxima-defuzzification">Mean of maxima</dfn>, <dfn>MOM</dfn>)</dt>
    <dd>Das Schwerpunktverfahren dient zur Defuzzifizierung.</dd>
    <dt><dfn id="informationsfusion-fuzzy">Fuzzy-Fusion</dfn></dt>
    <dd>

        <ol>
            <li>Definition linguistischer Variablen (z.B. Temperatur) und Terme (Werte der linguistischen Variablen, z.B. kalt, kühl, lau, warm, heiß)</li>
            <li>Zugehörigkeitsfunktionen definieren</li>
            <li>Fuzzifizierung: Transformation der vorliegenden Information mithilfe der Zugehörigkeitsfunktionen in Fuzzy-konforme Form. Zugehörigkeitsfunktionen bilden numerische Terme auf linguistische Variablen ab.</li>
            <li>Kombination der Terme durch Anwendung von Fuzzy-Logik in der Regelbasis. Die Regeln haben die Form IF Prämisse THEN Konklusion.</li>
            <li>Defuzzifizierung: Abbildung auf Ausgangsbasis (Schwerpunktregel, Maximummethode oder Maximum-Mittelwert Methode)</li>
        </ol>

    </dd>
</dl>


### Neuronale Netze

Slides: `IF-Kap5_160125.pdf`

Siehe [Vorlesung Neuronale Netze](https://martin-thoma.com/neuronale-netze-vorlesung/)


### Registrierung

Slides: `IF-Kap6_160125.pdf`

Wurde nicht besprochen.


### Energie&shy;funktionale

Slides: `IF-Kap7_160125.pdf`

<dl>
    <dt><a href="https://de.wikipedia.org/wiki/Funktional"><dfn>Funktional</dfn></a></dt>
    <dd>Ein <i>Funktional</i> ist eine Funktion aus einem Vektorraum $V$ in
        den Körper, der dem Vektorraum zugrunde liegt. Oft ist $V$ ein
        Funktionenraum, also ein Vektorraum, dessen Elemente reell- oder
        komplexwertige Funktionen sind. Ein Funktional ist somit eine Funktion
        auf Funktionen.</dd>
    <dt><dfn id="energiefunktionale">Energiefunktionale</dfn></dt>
    <dd>Durch die Einführung von Energietermen $E_k$ lassen sich
        fusionsrelevante Informationen modellieren. Die Fusionsaufgabe wird
        dann durch das Energiefunktional

        $$E = \sum_k \lambda_k E_k, \qquad \lambda_k > 0$$

        repräsentiert. Die unterschiedliche Relevanz der Energieterme $E_k$
        wird durch die Vorfaktoren $\lambda_k$ berücksichtigt.</dd>
    <dt><a href="https://en.wikipedia.org/wiki/Boltzmann_distribution"><dfn id="gibbs-distribution">Gibbs-Verteilung</dfn></a></dt>
    <dd>Die Gibbs-Verteilung mit dem Energiefunktional $E$ ist
        $$\pi_{\beta, E}(x) = \frac{1}{Z} e^{- \beta E(x)},$$
        mit Normierungskonstante $Z$ und der inversen Temperatur
        $\beta = \frac{1}{T}$.<br/>
        <br/>
        <u>Interpretation</u>:
        Es sei $E: V \rightarrow \mathbb{R}$, $V$ endlich.
        <ul>
            <li>$V$: Die Menge aller Konfigurationen eines physikalischen Systems.</li>
            <li>$E(x)$: Energie des Systems, wenn es sich in der Konfiguration $x$ befindet.</li>
            <li>$T$: Temperatur. Ist die Temperatur groß, so sind alle
                Konfigurationen etwa gleich wahrscheinlich. Bei niedriger
                Temperatur werden Konfigurationen mit niedriger Energie
                bevorzugt.</li>
            <li>$\pi_{\beta, E}(x)$: Wahrscheinlichkeit, dass sich das System
                in der Konfiguration $x$ befindet.</li>
        </ul>

        Für $E$ kann dann eine Gibbsche Wahrscheinlichkeitsdichtefunktion WDF
        $$WDF \propto e^{- \beta E} = \prod_k e^{-\frac{\lambda_k E_k}{T}}$$
        definiert werden.
        </dd>
    <dt><dfn>Energieminimierung</dfn></dt>
    <dd>

        <ol>
            <li>Lösung eines linearen Gleichungssystems (selten möglich)</li>
            <li>Graph-Cuts-Verfahren</li>
            <li>Approximative Lösung durch sukzessive Optimierung</li>
            <li>Methode des steilsten abstiegs</li>
            <li>Monte-Carlo-Methode</li>
            <li>Simulated Annealing</li>
            <li>Lineare Programme</li>
            <li>Dynamische Programmierung</li>
            <li>Mean Field Theorie (Betrachte Erwartungswerte)</li>
        </ol>

    </dd>
</dl>

## Überblick

<table>
    <tr>
        <th>Unsicherheitsmodellierung</th>
        <th>Wahrscheinlichkeiten</th>
        <th>Verallgemeinerte W-Keiten</th>
        <th>Linguistisch</th>
        <th>Neuronale Netze</th>
    </tr>
    <tr>
        <th>Fusion</th>
        <td><a href="#bayes-fusion">Bayes-Fusion</a></td>
        <td><a href="#drc">DRC</a></td>
        <td><a href="#informationsfusion-fuzzy">Fuzzy-Fusion</a></td>
        <td></td>
    </tr>
    <tr>
        <th>Unsicherheiten</th>
        <td>Wahrscheinlichkeit in [0, 1]</td>
        <td>Basismaß in [0, 1]</td>
        <td>Zugehörigkeit in [0, 1]</td>
        <td></td>
    </tr>
</table>


## Abkürzungen

* EKF: Extended Kalman Filter
* GUM: Guide to the Expression of Uncertainty in Measurement
* KF: Kalman Filter
* LS: Least Squares
* UKF: Unscented Kalman Filter


## Meine Fragen

* Kapitel 1, Folie 61: Was ist der Definitions / Wertebereich von Information?
* Kapitel 2, Folie 5: Alle Ereignisse paarweise disjunkt
* Kapitel 2, Folie 22: Man muss für wirksame Schätzer noch fordern, dass sie
                       erwartungstreu sind. Es gibt immer den konstanten
                       Schätzer, welcher die Stichprobe ignoriert und somit
                       eine Varianz von 0 hat.
* Kapitel 2, Folie 37: Was ist ein Arbeitspunkt?
* Kapitel 2, Folie 44f: Fusion 2er Größen / Verteilungen
* Kapitel 2, Folie 79: Was ist der Trunkation error? Was ist der base point error und warum ist es ein Problem, dass man um den Schätzwert und nicht um den wahren Wert linearisiert?


## Übungsaufgaben

Die Lösungen sind auch online (ausführlicher und besser als ich es hier habe).

### ÜB 1

* Aufgabe 1.1: http://math.stackexchange.com/q/1919394/6876
* Aufgabe 1.2: $P(A) = 0.5 = P(B) = P(C)$,
  $$
  \begin{align}
  P(A) \cdot P(B) &= 0.25 = P(A \cap B)\\
  P(A) \cdot P(C) &= 0.25 = P(A \cap C)\\
  P(A) \cdot P(B) \cdot P(C) &= 0.125 \neq 0.25 = P(A \cap B \cap C)
  \end{align}
  $$
  Daher sind die Ereignisse $A$ und $B$, die Ereignisse $A, C$, die Ereignisse
  $B, C$ unabhängig. Die Ereignisse $A, B, C$ sind jedoch nicht unabhängig.
* Aufgabe 1.3a: $5 \cdot (\frac{1}{6} \cdot \frac{1}{6}) = \frac{5}{36}$
* Aufgabe 1.3b: $\frac{\frac{2}{5} \cdot \frac{5}{36}}{1-0.25} = \frac{2}{27}$
* Aufgabe 1.4:
    * $P(X = 2) = P(X=12) = \frac{1}{36}$
    * $P(X = 3) = P(X=11) = \frac{2}{36}$
    * $P(X = 4) = P(X=10) = \frac{3}{36}$
    * $P(X = 5) = P(X=9) = \frac{4}{36}$
    * $P(X = 6) = P(X=8) = \frac{5}{36}$
    * $P(X = 7) = \frac{6}{36}$
    * $F(x) = \sum_{i=2}^x P(X = i)$
    * $\mathbb{E}(X) = 2 \cdot 3.5 = 7$
* Aufgabe 1.5a: $\int_0^\infty (\alpha \cdot \exp(-\alpha x)) \mathrm{d}x = \alpha \int_0^\infty \exp(-\alpha x) \mathrm{d}x = \alpha [-\frac{1}{\alpha} \exp(-\alpha x)]_0^\infty = 1$
* Aufgabe 1.5b: Erwartungswert einer Zufallsvariable ist $\int_{-\infty}^{+\infty} x f(x) \mathrm{d} x$.
* Aufgabe 1.6:
    * $G$: E-mail ist geschäftlich, $\bar{G}$ ist privat
    * $S$: E-mail ist spam, $\bar{S}$ ist ham
    * $F$: E-mail enthält das Wort "Free"
    * $P(S | F) = \frac{P(F | S) \cdot P(S)}{P(F)} = \frac{0.9 \cdot 0.7}{0.9 \cdot 0.7 + 0.01 \cdot 0.3} = \frac{210}{211}$


### ÜB 2

* Aufgabe 1.1: Die kontinuierliche Entropie ist kein resultat immer feiner werdender Diskretisierungen der diskreten Entropie
* Aufgabe 1.2a: $P(B \cap L) = P(B) \cdot P(L) = 1/3 \cdot 1/3 = 1/9$
* Aufgabe 1.2b: 1/9
* Aufgabe 1.2c: Das Prinzip der maximalen Entropie für Zufallsvariablen führt zur Unabhängigkeitsannahme.
* Aufgabe 1.2d: Die Entropie wird im Erwartungswert reduziert, wenn eine Größe durch eine andere bedingt wird.
* Aufgabe 1.3a: $P(z=z_A| d_A) = 0.45$, $P(z=z_B| d_A) = 0.45$, $P(z=z_K| d_A) = 0.1$
* Aufgabe 1.3b: Ist hier ein Zahlendreher passiert?
* Aufgabe 1.4a: Mit "Detektionsleistung" ist gemeint, wie wahrscheinlich der
                Sensor ein Objekt detektiert, wenn eines da ist. Mit
                "Klassifikationsleistung" ist gemeint, wie Wahrscheinlich
                der Sensor bei vorhandenem Objekt dieses richtig klassifiziert.
* Aufgabe 1.4b: Zentralisierte Bayessche Fusion (Likelihoodmatrizen)
* Aufgabe 1.4c: Zentralisierte Bayessche Fusion (A-posteriori-Verteilung)
* Aufgabe 1.4d: Verteilte Fusion
* Aufgabe 1.5: Berechnung der Log-A-posteriori-Verteilung
* Aufgabe 1.6: 0.043 (Das typische Patenten-Test-Beispiel)

### ÜB 3

* Aufgabe 1.1:
    * $Bel(A) = \sum_{B \subseteq A} m(B)$
    * $Pl(A) = 1 - Bel(\bar{A}) = \sum_{B \cap A \neq \emptyset} m(B)$
* Aufgabe 1.2a: Obwohl beide Basismaße dem Ereignis A eine sehr niedriges Maß
  zuweisen, ist es durch DRC das Ereignis mit dem höchsten Wert. Das liegt
  daran, dass die anderen jeweils exakt 0 haben.
* Aufgabe 1.2b: Jeder einzelne Experte gab A nur geringen glauben. Dennoch wird A deutlich am meisten Glauben nach der Fusion geschenkt.
* Aufgabe 1.2c: Das gleiche Ergebnis.
* Aufgabe 1.3a: $m_{123}(111) = 0.82$, das Ergebnis ist also zu 82% glaubwürdig. (Schönes beispiel, dass DRC nicht idempotent ist)
* Aufgabe 1.3b: Rechnen mit Basismaßen / DRC
* Aufgabe 1.3c: Rechnen mit Basismaßen / DRC
* Aufgabe 1.4: $P(s=A | w=A) + P(s=B | w=B) = 0.6$
* Aufgabe 2.1: Spielen mit Fuzzy-Mengen
* Aufgabe 2.2: XOR für Fuzzy-Mengen


## Prüfungsfragen

* Welche Bedingungen müssen erfüllt sein, damit Informationen fusioniert werden
  können?<br/>
  → Gemeinsamer Sachverhalt; kompatible Definitions- und Wertebereiche; Unsicherheitsbehaftet
* Welche Arten von Unsicherheit kennen Sie?<br/>
  → Unsicherheit kann man mit Wahrscheinlichkeiten, Basismaße (Dempster-Shafer-Theorie)
     und über unscharfe Mengen (Fuzzy-Systeme) sowie über unsicheres Erfahrungswissen
     (Neuronale Netze) beschreiben.
* Was ist der Unterschied zwischen Wahrscheinlichkeiten und Basismaßen?<br/>
  → Basismaße sind nicht monoton und nicht additiv.
* Wie lautet die Formel für verteilte Bayessche Fusion?<br/>
  → Wie zentrale bayessche Fusion, nur dass die Likelihood-Funktionen vorab berechnet werden (zweimaliges Anwenden der Bayes-Rule mit Unabhängigkeitsannahme zwischendurch)
* Wie lauten die Axiome von Kolmogorov?<br/>
  → Siehe <a href="#kolmogorov-axioms">oben</a>
* Was sind Zugehörigkeitsfunktionen?<br/>
  → Siehe <a href="#membership-function">oben</a>
* Wie funktioniert Informationsfusion mit Fuzzy-Systemen?<br/>
  → Siehe <a href="#informationsfusion-fuzzy">oben</a>.
* Welche Vorteile bietet Informationsfusion?<br/>
  → Siehe <a href="#informationsfusion-advantages">oben</a>.
* Welche Eigenschaften sind bei Merkmalen wünschenswert?<br/>
  → Siehe <a href="#features-properties">oben</a>.
* Welche Beziehung gilt zwischen Erwartungstreue und Konsistenz von Schätzern?<br/>
  → TODO



### Kalman-Filter

* Aus welchen Schritten besteht der Kalman-Filter?<br/>
  → Prädiktion, Innovation
* Welche Erweiterungen zum Kalman-Filter kennen Sie?<br/>
  → Extended Kalman Filter (EKF), UKF (Unscented Kalman Filter)
* Für welche Systeme ist der Kalman-Filter geeignet?<br/>
  → Lineare Zeitinvariante Systeme (LTI-Systeme)
* Wie entwickeln sich die Wahrscheinlichkeiten beim Kalman-Filter?<br/>
  → Bei der Prädiktion steigt die Unsicherheit, bei der Innovation sinkt sie.
* Wie lautet das Systemmodell im Kalman-Filter?<br/>
  → vgl. <a href="https://martin-thoma.com/kalman-filter/#step-2-modelling">Kalman-Filter Artikel</a>


## Absprachen

* Kapitel 5 (Neuronale Netze) und Kapitel 6 (Registrierung) kommen nicht dran.
* Übungsaufgaben sind auch Prüfungsrelevant.


## Material und Links

* [Vorlesungswebsite](http://ies.anthropomatik.kit.edu/lehre_informationsfusion.php)
* [Anki-Deck](./anki/Informationsfusion.apkg)
* [Anwendung der Dempster-Shafer Evidenztheorie auf die Bonitätsprüfung](https://statistik.econ.kit.edu/download/Artikel%20-%20Anwendung%20der%20Dempster-Shafer%20Evidenztheorie%20auf%20die%20Bonit%C3%A4tspr%C3%BCfung.pdf)
* [Mein Prüfungsprotokoll](https://github.com/MartinThoma/LaTeX-examples/blob/master/documents/kit-muendlich-informationsfusion/muendlich-we-2013-martin-thoma.pdf)



Literatur:

* **Bayes-Methoden**: James O. Berger: Statistical Decision Theory and Bayesian Analysis. 2nd edition, Springer, 2006. ISBN 0-387-96098-8.
* **Anwendungen**: Rick S. Blum, Zheng Liu (Hrsg.): Multi Sensor Image Fusion and Its Applications. Taylor & Francis, 2006. ISBN 0849334179.
* **Energiefunktionale**: James J. Clark, Alan L. Yuille: Data Fusion for Sensory Information Processing Systems. Kluwer Academic Publishers, 1990. ISBN 0792391209.
* **Fuzzy Logic**: Jochen Heinsohn; Rolf Socher-Ambrosius: Wissensverarbeitung: eine Einführung. Spektrum Akademischer Verlag, 1999. ISBN 3827403081.


## Vorlesungs&shy;empfehlungen

Folgende Vorlesungen sind ähnlich:

* [Analysetechniken großer Datenbestände](https://martin-thoma.com/analysetechniken-grosser-datenbestaende/)
* [Informationsfusion](https://martin-thoma.com/informationsfusion/)
* [Machine Learning 1](https://martin-thoma.com/machine-learning-1-course/)
* [Machine Learning 2](https://martin-thoma.com/machine-learning-2-course/)
* [Mustererkennung](https://martin-thoma.com/mustererkennung-klausur/)
* [Neuronale Netze](https://martin-thoma.com/neuronale-netze-vorlesung/)
* [Lokalisierung Mobiler Agenten](https://martin-thoma.com/lma/)
* [Probabilistische Planung](https://martin-thoma.com/probabilistische-planung/)


## Termine und Klausurablauf

Es ist eine mündliche Prüfung.

Ich habe meine am <b>Fraunhofer IOSB, Fraunhoferstr. 1, 76131 Karlsruhe</b>
am <b>11.10.2016</b> um <b>15:30 Uhr</b> bei Herrn Dr. Heizmann.
