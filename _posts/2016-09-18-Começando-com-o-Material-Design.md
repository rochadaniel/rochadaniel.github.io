---
layout: post
title:  "Começando com o Material Design"
date:   2016-09-18 03:43:45 +0700
categories: [android]
---

Você já deve ter ouvido falar do Material Design que foi introduzido na versão Lollipop do Android. No Material Design um monte de coisas novas foram introduzidas como **Material Theme**, **Novos Widgets**, **Sombras personalizadas**, **Vector Drawables** e **Animações personalizadas**.

Hoje você vai aprender a configurar um **Material Theme** e utilizar o componente **Toolbar**, substituto da **ActionBar**.

#### 1. Customizando as cores

O Material Design fornece um conjunto de propriedades para personalizar o tema da sua aplicação. Usamos cinco atributos primários para personalizar o tema geral.

`colorPrimaryDark` - Esta cor primária é mais escura do app e se aplica principalmente no fundo da barra de notificação.

`colorPrimary` - Esta é a cor principal do app. Ela será aplicada na barra de ferramentas (Toolbar - veremos mais sobre esse componente).

`textColorPrimary` - Esta é a cor principal do texto. Isso se aplica no título da barra de ferramentas.

`windowBackground` - Esta é a cor padrão de fundo.

`navigationBarColor` - Esta cor define a cor da barra de navegação do rodapé.

![](http://www.androidhive.info/wp-content/uploads/2015/04/android-material-design-color-schema.png)

> O google oferece essa [Palleta de Cores](http://www.google.co.in/design/spec/style/color.html#color-color-palette) para ajudar a escolher uma cor legal para o seu app :)

#### 2. Criando um Material Design Theme

**1.** No Android Studio, vá em **File ⇒ New Project** e preencha todos os detalhes para criar um novo projeto. Quando terminar, selecione **Blank Activity** e continue.

**2.** Abra **res ⇒ values ⇒ strings.xml** e adicione as seguintes linhas de código:

```xml
<resources>
    <string name="app_name">Material Design</string>
    <string name="action_settings">Settings</string>
    <string name="action_search">Search</string>
    <string name="drawer_open">Open</string>
    <string name="drawer_close">Close</string>

    <string name="nav_item_home">Home</string>
    <string name="nav_item_friends">Friends</string>
    <string name="nav_item_notifications">Messages</string>

    <!-- navigation drawer item labels  -->
    <string-array name="nav_drawer_labels">
        <item>@string/nav_item_home</item>
        <item>@string/nav_item_friends</item>
        <item>@string/nav_item_notifications</item>
    </string-array>

    <string name="title_messages">Messages</string>
    <string name="title_friends">Friends</string>
    <string name="title_home">Home</string>
</resources>
```
**3.** Abra **res ⇒ values ⇒ colors.xml** e adicione a seguintes linhas de código (Caso não encontre o arquivo colors.xml, crie um novo "resource file" com esse nome):

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#F50057</color>
    <color name="colorPrimaryDark">#C51162</color>
    <color name="textColorPrimary">#FFFFFF</color>
    <color name="windowBackground">#FFFFFF</color>
    <color name="navigationBarColor">#000000</color>
    <color name="colorAccent">#FF80AB</color>
</resources>
```

**4.** Abra **res ⇒ values ⇒ dimens.xml** e adicione as seguintes linhas de código:

```xml
<resources>
    <!-- Default screen margins, per the Android Design guidelines. -->
    <dimen name="activity_horizontal_margin">16dp</dimen>
    <dimen name="activity_vertical_margin">16dp</dimen>
    <dimen name="nav_drawer_width">260dp</dimen>
</resources>
```

**5.** Abra o arquivo **styles.xml** dentro de **res ⇒ values** e adicione as seguintes linhas de código. Os estilos definidos em **styles.xml** são comuns para todas as versões do Android. Vamos chamar nosso estiloe de **MyMaterialTheme**.

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

**6.** Agora dentro de **res**, crie uma pasta chamada **values-v21**. Dentro dela, crie outro arquivo **styles.xml** com os seguintes estilos. Esses estilos são especificos para versões superiores ao **Android Lollipop**.

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

**7.** Agora nós temos nosso estilo Material Design preparado. Now we have the basic Material Design styles ready. Agora para aplicar o tema no nosso aplicativo, vamos abrir o arquivo **AndroidManifest.xml** e modificar o atributo **android:theme** que fica dentro da tag **application**.

```xml
android:theme="@style/MyMaterialTheme"
```

Depois de aplicar o tema, seu arquivo **AndroidManifest.xml** vai ficar parecido com:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="info.androidhive.materialdesign" >

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/MyMaterialTheme" >
        <activity
            android:name=".activity.MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

Agora, se você rodar o seu app, verá a barra de notificação na cor especificada no seu estilo.

![](http://www.androidhive.info/wp-content/uploads/2015/04/android-material-design-notification-bar.png)

#### 3. Adicionando a Toolbar (Antiga ActionBar)

Usar **Toolbar** é facil e bastante customizavel. Podemos criar direto no arquivo **.xml** do nosso layout mas como a maioria das Toolbars do nosso app são iguais (mudando apenas suas actions, que são seu botões) eu gosto de criar um arquivo separado para nossa Toolbar e utilizar o atributo **include** em cada tela, visando deixar o código o mais limpo e reaproveitavel possivel.

**8.** Crie um arquivo xml com o nome **toolbar.xml** dentro de **res ⇒ layout** e adicione o elemento `android.support.v7.widget.Toolbar`.

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
**9.** Abra o arquivo de layout da sua MainActivity (provavelmente **activity_main.xml**) e adicione a **Toolbar** usando a tag **include**.

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

![](http://www.androidhive.info/wp-content/uploads/2015/04/android-material-design-toolbar1.png)

Agora vamos tentar adicionar uma toolbar com titulo e icones.

**10.** Faça download do [icone](https://design.google.com/icons/#ic_search) e faça a copia para a pasta **drawable**

----imagem-----

**11.** Agora abra o arquivo **menu_main.xml** que fica dentro de **res ⇒ menu** e adicione os seguintes itens:

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    tools:context=".MainActivity">

    <item
        android:id="@+id/action_search"
        android:title="@string/action_search"
        android:orderInCategory="100"
        android:icon="@drawable/ic_action_search"
        app:showAsAction="ifRoom" />

    <item
        android:id="@+id/action_settings"
        android:title="@string/action_settings"
        android:orderInCategory="100"
        app:showAsAction="never" />
</menu>
```

**12.** Agora abra a **MainActivity.java** e faça as seguintes alterações:

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

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }
}
```

Depois dessas alteracões, você deverá ver sua Toolbar da seguinte forma:

----imagens----



**Referências:**

* [Documentação oficial](https://developer.android.com/training/material/index.html)

* [Material Design Specifications](https://material.google.com/#)

* [Android Hive](http://www.androidhive.info/2015/04/android-getting-started-with-material-design/)
