---
layout: post
title:  "Android Trabalhando com Tabs"
date:   2016-09-21 07:43:45 +0000
categories: [android]
---
A biblioteca [Android Design Support Library](http://android-developers.blogspot.com.br/2015/05/android-design-support-library.html) facilita a portabilidade para apps >Android 2.1. Foram introduzidos nessa biblioteca componentes como **Navigation Drawer**, **Floating Action Button**, **Snackbar**, **Tabs** e **Floating labels**. Vamos aprender um por um, começando pelas Tabs.

[Baixe o código aqui]()

## 1. Implementando Tabs

**1.** Abra **res ⇒ values ⇒ colors.xml** e adicione a seguintes linhas de código (Caso não encontre o arquivo colors.xml, crie um novo "resource file" com esse nome):

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
**2.** Abra o arquivo **styles.xml** dentro de **res ⇒ values** e adicione as seguintes linhas de código. Os estilos definidos em **styles.xml** são comuns para todas as versões do Android. Vamos chamar nosso estilo de **MyMaterialTheme**.

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

**3.** Agora dentro de **res**, crie uma pasta chamada **values-v21**. Dentro dela, crie outro arquivo **styles.xml** com os seguintes estilos. Esses estilos são especificos para versões superiores ao **Android Lollipop**.

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

**4.** Agora nós temos nosso estilo Material Design preparado. Para aplicar o tema no nosso aplicativo, vamos abrir o arquivo **AndroidManifest.xml** e modificar o atributo **android:theme** que fica dentro da tag **application**.

```xml
android:theme="@style/MyMaterialTheme"
```

Depois de aplicar o tema, seu arquivo **AndroidManifest.xml** vai ficar parecido com:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="danielrocha.androidtabs">

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

**5.** Abra o arquivo **build.gradle** e adicione a biblioteca `com.android.support:design:23.4.0`

```java
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:23.4.0'
    compile 'com.android.support:design:23.4.0'
}
```

**6.** Adicione as seguintes dimensões no arquivo **dimens.xml** que fica dentro de **res ⇒ values**.

```xml
<resources>
    <!-- Default screen margins, per the Android Design guidelines. -->
    <dimen name="activity_horizontal_margin">16dp</dimen>
    <dimen name="activity_vertical_margin">16dp</dimen>
    <dimen name="tab_max_width">264dp</dimen>
    <dimen name="tab_padding_bottom">16dp</dimen>
    <dimen name="tab_label">14sp</dimen>
    <dimen name="custom_tab_layout_height">72dp</dimen>
</resources>
```

Antes de começarmos a criar nossas **Tabs**, vamos criar nossos **Fragments**. Cada um desses Fragments será responsável pela UI de cada Tab.

**7.** Dentro do seu pacote principal crie um Fragment chamado **OneFragment.java** (crie uma classe java normal) e adicione a seguinte linha de código:

```java
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import danielrocha.androidtabs.R;

/**
 * Created by danielrocha on 21/09/16.
 */
public class OneFragment extends Fragment {

    public OneFragment() {
        // Required empty public constructor
    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        View view = inflater.inflate(R.layout.fragment_one, container, false);

        ((TextView) view.findViewById(R.id.fragmentText)).setText("One");

        return view;
    }

}
```

**8.** Abra o arquivo **fragment_one.xml** dentro de **res ⇒ layout** e adicione o seguinte código:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:tools="http://schemas.android.com/tools"
    tools:context="danielrocha.androidtabs.OneFragment">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/fragmentText"
        android:textSize="40dp"
        android:textStyle="bold"
        android:layout_centerInParent="true"/>

</RelativeLayout>
```

**9.** Faça o mesmo procedimento de criar o Fragment e seu Layout para quantas tabs forem necessárias.

> No caso do nosso exemplo iremos usar um unico layout para todos os fragments, mudando apenas o texto de acordo com a sua posição

## 2. Tabs Fixas

Tabs Fixas devem ser usadas quando temos um numero fixo de Tabs.

**10.** Abra o arquivo **activity_main.xml** e adicione o código:

```xml
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">

        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            app:layout_scrollFlags="scroll|enterAlways"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />

        <android.support.design.widget.TabLayout
            android:id="@+id/tabs"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabMode="fixed"
            app:tabGravity="fill"/>
    </android.support.design.widget.AppBarLayout>

    <android.support.v4.view.ViewPager
        android:id="@+id/viewpager"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"  />
</android.support.design.widget.CoordinatorLayout>
```

> Esse código faz uso de componentes da "Android Design Support Library" que ainda não vimos no Blog (CoordinatorLayout, AppBarLayout e TabLayout) mas assim que possivel vou estar disponibilizando alguns estudos sobre eles.

**11.** Abra o arquivo **MainActivity.java** e faça as seguintes modificações:

* `tabLayout.setupWithViewPager()` - Atribui o ViewPager ao TabLayout

* `setupViewPager()` - Define o numero de tabs e o titulo de cada tab para cada Fragment

* `ViewPagerAdapter` - Adapter que fornece os eventos necessários para o ViewPager

```java
import android.os.Bundle;
import android.support.design.widget.TabLayout;
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentPagerAdapter;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;

import java.util.ArrayList;
import java.util.List;

import danielrocha.androidtabs.fragments.OneFragment;
import danielrocha.androidtabs.fragments.ThreeFragment;
import danielrocha.androidtabs.fragments.TwoFragment;

public class MainActivity extends AppCompatActivity {

    private Toolbar toolbar;
    private TabLayout tabLayout;
    private ViewPager viewPager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        viewPager = (ViewPager) findViewById(R.id.viewpager);
        setupViewPager(viewPager);

        tabLayout = (TabLayout) findViewById(R.id.tabs);
        tabLayout.setupWithViewPager(viewPager);
    }

    private void setupViewPager(ViewPager viewPager) {
        ViewPagerAdapter adapter = new ViewPagerAdapter(getSupportFragmentManager());
        adapter.addFragment(new OneFragment(), "ONE");
        adapter.addFragment(new TwoFragment(), "TWO");
        adapter.addFragment(new ThreeFragment(), "THREE");
        viewPager.setAdapter(adapter);
    }

    class ViewPagerAdapter extends FragmentPagerAdapter {
        private final List<Fragment> mFragmentList = new ArrayList<>();
        private final List<String> mFragmentTitleList = new ArrayList<>();

        public ViewPagerAdapter(FragmentManager manager) {
            super(manager);
        }

        @Override
        public Fragment getItem(int position) {
            return mFragmentList.get(position);
        }

        @Override
        public int getCount() {
            return mFragmentList.size();
        }

        public void addFragment(Fragment fragment, String title) {
            mFragmentList.add(fragment);
            mFragmentTitleList.add(title);
        }

        @Override
        public CharSequence getPageTitle(int position) {
            return mFragmentTitleList.get(position);
        }
    }
}
```
Após rodar o app, você deve ver uma tela parecida com essa:

![](/static/img/posts/tabs/tabs1.png)

## 2.1 Tabs ocupando tela inteira

Se você quer que as suas Tabs ocupem toda a largura da tela, sete `app:tabGravity=”fill"` no seu **TabLayout**.

![](/static/img/posts/tabs/tabs2.png)

## 2.2 Tabs centralizadas

Se você quer as suas Tabs centralizadas na tela, sete`app:tabGravity=”center”` no seu **TabLayout**.

![](/static/img/posts/tabs/tabs3.png)

## 2.3 Scrollable Tabs

Scrollable Tabs devem ser usadas quando temos um numero grande de tabs, que provavelmente não caberão na tela do aparelho. Para fazer o teste, sete `app:tabMode=”scrollable”` no seu **TabLayout** e adicione em torno de 10 Fragments para poder visualizar o efeito do scroll.

![](/static/img/posts/tabs/tabs4.png)

## 3. Tabs com icone e texto

Tudo o que deve ser feito é chamar o método `setIcon()` passando como parametro o icone escolhido.

```
tabLayout.getTabAt(0).setIcon(tabIcons[0]);
tabLayout.getTabAt(1).setIcon(tabIcons[1]);
```

**12.** Abra o arquivo **MainActivity.java** e modifique da seguinte forma. Repare que criamos um vetor de icones `int[] tabIcons` e criamos o método `setupTabIcons()`:

```java
package danielrocha.androidtabs;

import android.os.Bundle;
import android.support.design.widget.TabLayout;
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentPagerAdapter;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    private Toolbar toolbar;
    private TabLayout tabLayout;
    private ViewPager viewPager;
    private int[] tabIcons = {
            R.drawable.ic_favorite_white_24dp,
            R.drawable.ic_call_white_24dp,
            R.drawable.ic_contacts_white_24dp
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        viewPager = (ViewPager) findViewById(R.id.viewpager);
        setupViewPager(viewPager);

        tabLayout = (TabLayout) findViewById(R.id.tabs);
        tabLayout.setupWithViewPager(viewPager);
        setupTabIcons();
    }

    private void setupTabIcons() {
        tabLayout.getTabAt(0).setIcon(tabIcons[0]);
        tabLayout.getTabAt(1).setIcon(tabIcons[1]);
        tabLayout.getTabAt(2).setIcon(tabIcons[2]);
    }

    private void setupViewPager(ViewPager viewPager) {
        ViewPagerAdapter adapter = new ViewPagerAdapter(getSupportFragmentManager());
        adapter.addFrag(new OneFragment(), "ONE");
        adapter.addFrag(new TwoFragment(), "TWO");
        adapter.addFrag(new ThreeFragment(), "THREE");
        viewPager.setAdapter(adapter);
    }

    class ViewPagerAdapter extends FragmentPagerAdapter {
        private final List<Fragment> mFragmentList = new ArrayList<>();
        private final List<String> mFragmentTitleList = new ArrayList<>();

        public ViewPagerAdapter(FragmentManager manager) {
            super(manager);
        }

        @Override
        public Fragment getItem(int position) {
            return mFragmentList.get(position);
        }

        @Override
        public int getCount() {
            return mFragmentList.size();
        }

        public void addFrag(Fragment fragment, String title) {
            mFragmentList.add(fragment);
            mFragmentTitleList.add(title);
        }

        @Override
        public CharSequence getPageTitle(int position) {
            return mFragmentTitleList.get(position);
        }
    }
}
```

> **Baixe os icones aqui:** [ic_tab_favourite](https://design.google.com/icons/#ic_favorite), [ic_tab_call](https://design.google.com/icons/#ic_call), [ic_tab_contacts](https://design.google.com/icons/#ic_contacts)

![](/static/img/posts/tabs/tabs5.png)

## 4. Tabs com apenas icones

Modifique o método `getPageTitle()` dentro do nosso adapter (**ViewPagerAdapter**).

```java
@Override
public CharSequence getPageTitle(int position) {

    // return null to display only the icon
    return null;
}
```
![](/static/img/posts/tabs/tabs6.png)

## 5. Custom Tab View

Customizar as tabs pode ser algo necessário dependendo do seu app, mas antes disso temos que ter certeza que está dentro das [especificações](https://material.google.com/components/tabs.html#tabs-specs) sugeridas pelo Google.

Quando fizemos o exemplo de Tabs com Icone + Texto, vimos que o Icone vem ao lado do texto, agora vamos deixar o Icone acima do texto.

**13.** Abra o arquvio **activity_main.xml** e sete a altura do TabLayout. Setar a altura é importante para usar um espaço maior do que o normal.

```xml
<android.support.design.widget.TabLayout
           android:id="@+id/tabs"
           android:layout_width="match_parent"
           android:layout_height="@dimen/custom_tab_layout_height"
           app:tabMode="fixed"
           app:tabGravity="fill"/>
```

**14.** Crie um arquivo chamado **custom_tab.xml** dentro de **res ⇒ layout**. Nesse layout vai ser definido a view que ficará dentro da tab, no nosso caso iremos usar apenas um **TextView**.

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/tab"
    android:textColor="@color/colorAccent"
    android:textSize="@dimen/tab_label"/>
```

**15.** Abra o arquivo **MainActivity.java** e modifique o método `setupTabIcons()` de acordo com o código abaixo. Observe que renderizamos o layout **custom_tab.xml** em cada tab.

```java
private void setupTabIcons() {
  TextView tabOne = (TextView) LayoutInflater.from(this).inflate(R.layout.custom_tab, null);
  tabOne.setText("ONE");
  tabOne.setCompoundDrawablesWithIntrinsicBounds(0, tabIcons[0], 0, 0);
  tabLayout.getTabAt(0).setCustomView(tabOne);

  TextView tabTwo = (TextView) LayoutInflater.from(this).inflate(R.layout.custom_tab, null);
  tabTwo.setText("TWO");
  tabTwo.setCompoundDrawablesWithIntrinsicBounds(0, tabIcons[1], 0, 0);
  tabLayout.getTabAt(1).setCustomView(tabTwo);

  TextView tabThree = (TextView) LayoutInflater.from(this).inflate(R.layout.custom_tab, null);
  tabThree.setText("THREE");
  tabThree.setCompoundDrawablesWithIntrinsicBounds(0, tabIcons[2], 0, 0);
  tabLayout.getTabAt(2).setCustomView(tabThree);
}
```

![](/static/img/posts/tabs/tabs7.png)

Use a imaginação :)
