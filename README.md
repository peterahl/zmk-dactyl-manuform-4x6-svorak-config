# ZMK Firmware Configuration for Dactyl Manuform (Svorak - 52 Keys)

Detta arkiv innehåller ZMK firmware-konfiguration för ett delat **Dactyl Manuform** mekaniskt tangentbord med
**nice!nano (v2)** mikrokontrollers och **Svorak** (svensk Dvorak) tangentbordsschema.

---

## 📌 Hårdvarulayout (52 Tangenter)

Ditt tangentbord har totalt **52 tangenter** (26 tangenter per halva):

- **Huvudgrid**: 3 rader × 6 kolumner (18 tangenter per halva).
- **Extra fingertangenter**: 2 tangenter placerade längst ner under lång- och ringfingrarna (kolumn 2 och kolumn 3).
- **Tumkluster**: 6 tumtangenter per halva.

---

## 🔌 Kopplingsdiagram & Pinout (Wiring Diagram)

Varje halva drivs av en **nice!nano v2** (Pro Micro footprint) och använder en matris med **5 rader** och **6 kolumner**
(totalt 26 tangenter per halva).

### Matriskoppling för nice!nano (Pro Micro Pinout)

| Matrisstift | Pro Micro Pin | nice!nano Pin | Beskrivning |
| :--- | :--- | :--- | :--- |
| **Row 0** | Pin 4 | P0.06 | Översta bokstavsraden |
| **Row 1** | Pin 5 | P0.08 | Hemraden (Home row) |
| **Row 2** | Pin 6 | P0.17 | Nedersta bokstavsraden |
| **Row 3** | Pin 7 | P0.20 | Extra tangenter nertill (lång- & ringfinger) |
| **Row 4** | Pin 8 | P0.22 | Tumkluster (6 tumtangenter) |
| **Col 0** | Pin 19 (RAW) | P0.31 | Kolumn 0 (längst till vänster) |
| **Col 1** | Pin 18 (A0) | P0.29 | Kolumn 1 |
| **Col 2** | Pin 15 (A1) | P0.02 | Kolumn 2 (Ringfinger) |
| **Col 3** | Pin 14 (A2) | P1.15 | Kolumn 3 (Långfinger) |
| **Col 4** | Pin 16 (A3) | P1.13 | Kolumn 4 |
| **Col 5** | Pin 10 | P0.10 | Kolumn 5 (längst till höger) |

### Övriga komponenter per halva

1. **Dioder**: 1N4148 koppling `col2row`. Diodens katod (sidan med det svarta strecket) löds mot radledningen, och
   anoden mot tangentbrytaren/kolumnen.
2. **Batteri (LiPo 3.7V)**:
   - Röd ledning (Positiv `+`) -> ansluts till `B+` på nice!nano (via en **On/Off-skjutomkopplare**).
   - Svart ledning (Negativ `-`) -> ansluts till `B-` på nice!nano.
3. **Reset-knapp**: En momentan tryckknapp kopplad mellan `RST` och `GND` på nice!nano.

---

## 🚀 Installation av Firmware på nice!nano

### Steg 1: Bygg `.uf2`-filerna

Vid varje push till `main`-branchen i detta GitHub-arkiv bygger GitHub Actions automatiskt firmware-filerna.

1. Gå till fliken **Actions** i ditt GitHub-repository.
2. Klicka på den senaste lyckade körningen.
3. Ladda ner artifact-arkivet som innehåller:
   - `zmk_x6_manuform_left_nice_nano_v2.uf2` (för vänster / central halva)
   - `zmk_x6_manuform_right_nice_nano_v2.uf2` (för höger / peripheral halva)

### Steg 2: Flasha Vänster Halva (Central)

1. Anslut den **vänstra** nice!nano till datorn med en USB-C-datakabel.
2. Dubbelklicka snabbt på reset-knappen på din nice!nano.
3. En ny enhet/disk med namnet **`NICENANO`** dyker upp i din filhanterare.
4. Dra och släpp (eller kopiera) `zmk_x6_manuform_left_nice_nano_v2.uf2` till `NICENANO`-disken.
5. Kortet kopplar automatiskt ifrån och startar om med det nya firmwaret.

### Steg 3: Flasha Höger Halva (Peripheral)

1. Anslut den **högra** nice!nano till datorn med USB-C-kabeln.
2. Dubbelklicka snabbt på reset-knappen på nice!nano tills **`NICENANO`**-disken dyker upp.
3. Dra och släpp `zmk_x6_manuform_right_nice_nano_v2.uf2` till `NICENANO`-disken.
4. Kortet startar om. Slå på batteriströmbrytaren på båda halvorna.

---

## 💻 USB vs Bluetooth-anslutning

- **Datoranslutning**: Den vänstra halvan är inställd som **Central**. När du sätter i USB-kabeln i den vänstra halvan
  prioriterar ZMK automatiskt **USB-utskrift** till datorn.
- **Trådlös split**: Höger halva skickar alla sina knapptryckningar trådlöst via BLE till den vänstra halvan, som sedan
  skickar vidare allt till datorn via USB.

### Växla utskriftsläge manuellt

Du kan tvinga utskrift till USB eller Bluetooth direkt via tangentbordet på **Lager 2 (Sifferlagret)**:

- `&out OUT_USB`: Tvingar tangentbordet att skicka utskrift via **USB**.
- `&out OUT_BLE`: Växlar utskrift till **Bluetooth**.
- `&out OUT_TOG`: Växlar mellan USB och BLE.
- `&bt BT_CLR`: Rensar Bluetooth-parning vid felsökning.

---

## ⌨️ Keymap Översikt (52 Tangenter)

### Lager 0: Bokstäver (Svorak Layout)

```text
[TAB]    [Å]   [Ä]   [Ö]   [P]   [Y]           [F]   [G]   [C]   [R]   [L]   [BSPC]
[LCTRL]  [A]   [O]   [E]   [U]   [I]           [D]   [H]   [T]   [N]   [S]   [-]
[LSHFT]  [,]   [.]   [J]   [K]   [X]           [B]   [M]   [W]   [V]   [Z]   [RSHFT]
               [LEFT][DOWN]                                [UP]  [RIGHT]
[BSPC]   [DEL] [ESC] [MO1] [MO2] [ALT]         [MO2] [MO1] [RCTRL] [SPACE] [RET] [TAB]
```

### Lager 1: Symboler (`&mo 1`)

```text
[`]      [!]   [@]   [#]   [$]   [%]           [^]   [&]   [*]   [(]   [)]   [DEL]
[TRANS]  [{]   [}]   [(]   [)]   [&]           [-]   [=]   [:]   [;]   [']   ["]
[TRANS]  [[]   []]   [<]   [>]   [|]           [\]   [+]   [,]   [.]   [/]   [TRANS]
               [HOME][END]                                 [PGUP][PGDN]
[TRANS]  [TRANS] [TRANS] [TRANS] [TRANS] [TRANS]   [TRANS] [TRANS] [TRANS] [SPACE] [RET] [TRANS]
```

### Lager 2: Siffror, Navigation & System (`&mo 2`)

```text
[ESC]    [F1]   [F2]   [F3]   [F4]   [F5]          [7]   [8]   [9]   [+]   [-]   [BSPC]
[TRANS]  [F6]   [F7]   [F8]   [F9]   [F10]         [4]   [5]   [6]   [*]   [/]   [RET]
[OUT_USB][OUT_BLE][OUT_TOG][BTCLR][BT0] [BT1]      [1]   [2]   [3]   [=]   [.]   [TRANS]
               [LEFT][DOWN]                                [UP]  [RIGHT]
[TRANS]  [TRANS] [TRANS] [TRANS] [TRANS] [TRANS]   [TRANS] [TRANS] [TRANS] [0]   [.]   [TRANS]
```
