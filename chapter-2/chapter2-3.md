## 2.3.  How to Specify Materials in the Detector
### 2.3.1.  General Considerations
本来は、一般的な物質（化合物、混合物）は元素からできていて、元素は同位体からできている。
それゆえgeant4で設計された3つの主なクラスがある。
これらのクラスのそれぞれは、それぞれのクラスで作られたインスタンスを追跡する静的なデータメンバーとしてテーブルを持っている。
3つのクラスはすべて生成の際に自動的に一致したテーブルに登録され、ユーザーの手で削除してはならない。

G4Elementクラスでは原子の特性を記述する。
-   原子番号
-   核子の数
-   質量数
-   殻エネルギー
-   断面積等の物理量

G4Materialクラスは巨視的な物質の特性を記述する。
-   密度
-   状態
-   温度
-   圧力
-   放射長、平均自由工程、エネルギー阻止能等の巨視的な物理量

G4Materialクラスは残りのツールキットに見えるものであり、トラッキング、形状、物理に用いられる。
それは、実装の詳細を隠すと同時に実際の物質の元素、同位体に関したすべての情報を含む。

### 2.3.2 Define a Sample Material
下の例では、名前、密度、モル質量、原子数を特徴づけることで液体のアルゴンが生成されている。
```C++
Example 2.7.  Creating liquid argon.
  G4double z, a, density;
  density = 1.390*g/cm3;
  a = 39.95*g/mole;

  G4Material* lAr = new G4Material(name="liquidArgon", z=18., a, density);

ロジカルボリュームに物質lArへのポインタを与えている。
G4LogicalVolume* myLbox = new G4LogicalVolume(aBox,lAr,"Lbox",0,0,0);
```

### 2.3.3 Define Molecule
下の例では水(H2O)が分子での原子数を明記することで作成されている。
```C++
Example 2.8.  Creating water by defining its molecular components.
  G4double z, a, density;
  G4String name, symbol;
  G4int ncomponents, natoms;

  a = 1.01*g/mole;
  G4Element* elH  = new G4Element(name="Hydrogen",symbol="H" , z= 1., a);

  a = 16.00*g/mole;
  G4Element* elO  = new G4Element(name="Oxygen"  ,symbol="O" , z= 8., a);

  density = 1.000*g/cm3;
  G4Material* H2O = new G4Material(name="Water",density,ncomponents=2);
  H2O->AddElement(elH, natoms=2);
  H2O->AddElement(elO, natoms=1);
```

### 2.3.4 Define a Mixture by Fractional Mass
下の例では、空気がそれぞれの要素の質量分立を明記することで窒素と酸素から構成されている。
```C++
Example 2.9.  Creating air by defining the fractional mass of its components.
  G4double z, a, fractionmass, density;
  G4String name, symbol;
  G4int ncomponents;

  a = 14.01*g/mole;
  G4Element* elN  = new G4Element(name="Nitrogen",symbol="N" , z= 7., a);

  a = 16.00*g/mole;
  G4Element* elO  = new G4Element(name="Oxygen"  ,symbol="O" , z= 8., a);

  density = 1.290*mg/cm3;
  G4Material* Air = new G4Material(name="Air  ",density,ncomponents=2);
  Air->AddElement(elN, fractionmass=70*perCent);
  Air->AddElement(elO, fractionmass=30*perCent);
```

### 2.3.5 Define a Material from the Geant4 Material Database
下の例では、空気と水の情報をgeant4のマテリアルデータベース経由で取得している。
```C++
Example 2.10.  Defining air and water from the internal Geant4 database.
  G4NistManager* man = G4NistManager::Instance();
  G4Material* H2O  = man->FindOrBuildMaterial("G4_WATER");
  G4Material* Air  = man->FindOrBuildMaterial("G4_AIR");
```

### 2.3.6 Define a Material from the Base Material
既存の物質をもとに新しい物質を生成することができる。下の例では通常でない密度を持つ水を生成する2つの方法を
示している。
```C++
Example 2.11.  Defining water with user defined density on base of G4_WATER.
  G4double density;

  density = 1.05*mg/cm3;
  G4Material* water1 = new G4Material("Water_1.05",density,"G4_WATER");

  density = 1.03*mg/cm3;
  G4NistManager* man = G4NistManager::Instance();
  G4Material* water2 = man->BuildMaterialWithNewDensity("Water_1.03","G4_WATER",density);
```

### 2.3.7 Print Material Information
```C++
Example 2.12.  Printing information about materials.
  G4cout << H2O;                                \\ print a given material
  G4cout << *(G4Material::GetMaterialTable());  \\ print the list of materials
```
```C++
2.3.8 Access to Geant4 naterial database
Example 2.13.  Geant4 material database may be accessed via UI commands.
  /material/nist/printElement  Fe      \\ print element by name
  /material/nist/printElementZ 13      \\ print element by atomic number
  /material/nist/listMaterials type    \\ print materials type = [simple | compound | hep | all]
  /material/g4/printElement    elmName \\ print instantiated element by name
  /material/g4/printMaterial   matName \\ print instantiated material by name
```
