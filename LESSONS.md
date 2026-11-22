# Lecciones de porte (para otro modelo)

## Qué falló
1. El JSON de Rising Power no es JSON válido: faltan comas y hay comentarios `//`. Un reparador por regex metió comas **dentro** de las cadenas (`[+3 Food], [in capital]`). Hay que tokenizar y no tocar el interior de strings.
2. Copiar `obsoleteTech`, `requiredTech` y `upgradesTo` tal cual. Esas techs y la unidad `Mounted Archer` solo existen en el ruleset base de RP, no en G&K.
3. Uniques de contabilidad interna del mod (`Number of Fighters`, `City Level`, `Number of Buildings`, `CD`, `Marriage Point`, `Civilized Leader-*`, eventos) no son recursos ni edificios de G&K. El validador las marca en rojo aunque lleven `<hidden from users>`.
4. `Costs [n] [Iron]` y `heals … <costs [n] [Iron]>` piden `stockpiledResource`. En G&K el hierro es recurso estratégico (`Consumes [1] [Iron]`), no stockpile.
5. Filtros de tipo propios (`Mobile`, `Anti-Mobile`, `Shooter`, `Reconnaissance`) no son `mapUnitFilter` de G&K.
6. Promociones con `row`/`column` del mod de origen pisan la rejilla de G&K (Charge I = Wolfpack I en 1/14, Additional Range = Wolfpack III en 3/14, Sealift = March en 3/10).
7. Quitar una promoción porque su unique no valía, sin quitar la referencia en la unidad, deja "promotion which does not exist".

## Cómo se arregló
- Mapa de techs: Defensive Fortification→Construction, Army Formation→Bronze Working, Materials Science→Plastics, Burial Custom/Belief Worship→Pottery, Enlightenment/Art→Acoustics, Agricultural Improvement→Economics, Nation→Philosophy.
- `Mounted Archer` (replaces y upgradesTo) → `Knight`.
- Se eliminan uniques con parámetros que no existen en G&K, marcadores `BUILDING`/`MILITARY`/`Seaside Building`/`Comment`, y costes de stockpile.
- Filtros: Mobile→Mounted, Anti-Mobile→Sword, Reconnaissance→Scout.
- Promociones propias van a columnas 27+ (G&K usa hasta la 26).
- Si el efecto original no es portable, se deja la promoción con un unique válido mínimo para que la referencia no rompa, y se anota en el registro.
- `hidden from users` no oculta el error del validador.
