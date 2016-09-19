---
layout: post
title:  "Começando com o Material Design"
date:   2016-09-18 03:43:45 +0700
categories: [android]
---

Você já deve ter ouvido falar do Material Design que foi introduzido na versão Lollipop do Android. No Material Design um monte de coisas novas foram introduzidas como **Material Theme**, **Novos Widgets**, **Sombras personalizadas**, **Vector Drawables** e **Animações personalizadas**.

Hoje você vai aprender a configurar um **Material Theme** e utilizar o componente **Toolbar**, substituto da **ActionBar**.

[Baixe o código aqui](https://github.com/rochadaniel/AndroidToolbar)

## 1. Customizando as cores

O Material Design fornece um conjunto de propriedades para personalizar o tema da sua aplicação. Usamos cinco atributos primários para personalizar o tema geral.

`colorPrimaryDark` - Esta cor primária é mais escura do app e se aplica principalmente no fundo da barra de notificação.

`colorPrimary` - Esta é a cor principal do app. Ela será aplicada na barra de ferramentas (Toolbar - veremos mais sobre esse componente).

`textColorPrimary` - Esta é a cor principal do texto. Isso se aplica no título da barra de ferramentas.

`windowBackground` - Esta é a cor padrão de fundo.

`navigationBarColor` - Esta cor define a cor da barra de navegação do rodapé.

![](/static/img/toolbar_example.png)

> O google oferece essa [Palleta de Cores](http://www.google.co.in/design/spec/style/color.html#color-color-palette) para ajudar a escolher uma cor legal para o seu app :)

## 2. Criando um Material Design Theme

**1.** No Android Studio, vá em **File ⇒ New Project** e preencha todos os detalhes para criar um novo projeto. Quando terminar, selecione **Blank Activity** e continue.

**2.** Abra **res ⇒ values ⇒ strings.xml** e adicione as seguintes linhas de código:

```xml
<resources>
    <string name="app_name">AndroidToolbar</string>
    <string name="action_settings">Settings</string>
    <string name="action_search">Search</string>
</resources>
```
**3.** Abra **res ⇒ values ⇒ colors.xml** e adicione a seguintes linhas de código (Caso não encontre o arquivo colors.xml, crie um novo "resource file" com esse nome):

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#3F51B5</color>
    <color name="colorPrimaryDark">#303F9F</color>
    <color name="colorAccent">#FF4081</color>
    <color name="textColorPrimary">#FFFFFF</color>
    <color name="windowBackground">#FFFFFF</color>
    <color name="navigationBarColor">#000000</color>
</resources>
```
**4.** Abra o arquivo **styles.xml** dentro de **res ⇒ values** e adicione as seguintes linhas de código. Os estilos definidos em **styles.xml** são comuns para todas as versões do Android. Vamos chamar nosso estiloe de **MyMaterialTheme**.

```xml
<resources>

    <style name="MyMaterialTheme" parent="MyMaterialTheme.Base">

    </style>

    <style name="MyMaterialTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="windowNoTitle">true</item>
        <item name="windowActionBar">false</item>
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

</resources>
```

**5.** Agora dentro de **res**, crie uma pasta chamada **values-v21**. Dentro dela, crie outro arquivo **styles.xml** com os seguintes estilos. Esses estilos são especificos para versões superiores ao **Android Lollipop**.

```xml
<resources>

    <style name="MyMaterialTheme" parent="MyMaterialTheme.Base">
        <item name="android:windowContentTransitions">true</item>
        <item name="android:windowAllowEnterTransitionOverlap">true</item>
        <item name="android:windowAllowReturnTransitionOverlap">true</item>
        <item name="android:windowSharedElementEnterTransition">@android:transition/move</item>
        <item name="android:windowSharedElementExitTransition">@android:transition/move</item>
    </style>

</resources>
```
Perceba que o nome do nosso estilo continua sendo **MyMaterialTheme**, sendo assim ele herda os estilos do nosso outro arquivo **styles.xml** e adiciona esses novos atributos quando a versão do aparelho for igual ou superior ao **Android Lollipop**.

**6.** Agora nós temos nosso estilo Material Design preparado. Now we have the basic Material Design styles ready. Agora para aplicar o tema no nosso aplicativo, vamos abrir o arquivo **AndroidManifest.xml** e modificar o atributo **android:theme** que fica dentro da tag **application**.

```xml
android:theme="@style/MyMaterialTheme"
```

Depois de aplicar o tema, seu arquivo **AndroidManifest.xml** vai ficar parecido com:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.danielrocha.androidtoolbar">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/MyMaterialTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

Agora, se você rodar o seu app, verá a barra de notificação na cor especificada no seu estilo.

![](/static/img/toolbar_example1.png)

## 3. Adicionando a Toolbar (Antiga ActionBar)

Usar **Toolbar** é facil e bastante customizavel. Podemos criar direto no arquivo **.xml** do nosso layout mas como a maioria das Toolbars do nosso app são iguais (mudando apenas suas actions, que são seu botões) eu gosto de criar um arquivo separado para nossa Toolbar e utilizar o atributo **include** em cada tela, visando deixar o código o mais limpo e reaproveitavel possivel.

**7.** Crie um arquivo xml com o nome **toolbar.xml** dentro de **res ⇒ layout** e adicione o elemento `android.support.v7.widget.Toolbar`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:local="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    local:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    local:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
```
**8.** Abra o arquivo de layout da sua MainActivity (provavelmente **activity_main.xml**) e adicione a **Toolbar** usando a tag **include**.

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:orientation="vertical">

        <include
            android:id="@+id/toolbar"
            layout="@layout/toolbar" />
    </LinearLayout>

</RelativeLayout>
```
Rode o aplicativo e veja se a toolbar será visualizada da seguinte forma:

![](/static/img/toolbar_example2.png)

Agora vamos tentar adicionar uma toolbar com titulo e icones.

**9.** Faça download do [icone](https://design.google.com/icons/#ic_search). Após baixar o arquivo, o icone será separado em algumas pastas (drawable-hdpi,mdpi,xhdpi,xxhdpi,xxxhdpi). Selecione essas pastas e copie para o diretório **res** que o Android Studio já separa os icones de acordo com a densidade de tela que o seu app irá rodar.

![](/static/img/toolbar_drawables.png)

**10.** Agora abra o arquivo **menu_main.xml** que fica dentro de **res ⇒ menu** e adicione os seguintes itens. Caso não exista, crie uma pasta dentro de **res** chamada **menu** e depois crie um arquivo com o nome **menu_main.xml**.:

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    tools:context=".MainActivity">

    <item
        android:id="@+id/action_search"
        android:title="@string/action_search"
        android:orderInCategory="100"
        android:icon="@drawable/ic_search_white_24dp"
        app:showAsAction="ifRoom" />

    <item
        android:id="@+id/action_settings"
        android:title="@string/action_settings"
        android:orderInCategory="100"
        app:showAsAction="never" />
</menu>
```

**11.** Agora abra a **MainActivity.java** e faça as seguintes alterações:

* Extenda de `AppCompatActivity`

* Habilitar a **Toolbar** para se comportar como a **ActionBar** chamando o método `setSupportActionBar()` passando a nossa Toolbar.

* Sobrescrever os métodos `onCreateOptionsMenu()` e `onOptionsItemSelected()` para habilitar as ações (clicks por exemplo) dos itens da **Toolbar**.

```java
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.view.Menu;
import android.view.MenuItem;

public class MainActivity extends AppCompatActivity {

    private Toolbar mToolbar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mToolbar = (Toolbar) findViewById(R.id.toolbar);

        setSupportActionBar(mToolbar);
        getSupportActionBar().setDisplayShowHomeEnabled(true);
    }


    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        switch (id) {
            case R.id.action_search:
                Toast.makeText(MainActivity.this, "Search click", Toast.LENGTH_SHORT).show();
                break;

            case R.id.action_settings:
                Toast.makeText(MainActivity.this, "Settings click", Toast.LENGTH_SHORT).show();
                break;
        }

        return super.onOptionsItemSelected(item);
    }
}
```
Repare que no método `onOptionsItemSelected` criamos um `switch` para pegar o retorno do click das actions na **Toolbar**.

Depois dessas alteracões, você deverá ver sua Toolbar da seguinte forma:

![](/static/img/toolbar_example3.png)

![](/static/img/toolbar_example4.png)

[Baixe o código aqui](https://github.com/rochadaniel/AndroidToolbar)

**Referências:**

* [Documentação oficial](https://developer.android.com/training/material/index.html)

* [Material Design Specifications](https://material.google.com/#)

* [Android Hive](http://www.androidhive.info/2015/04/android-getting-started-with-material-design/)
