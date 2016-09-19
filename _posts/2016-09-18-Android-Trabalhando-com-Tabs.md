---
layout: post
title:  "Android Trabalhando com Tabs"
date:   2016-09-18 13:43:45 +0000
categories: [android]
---
A biblioteca [Android Design Support Library](http://android-developers.blogspot.com.br/2015/05/android-design-support-library.html) facilita a portabilidade para apps >Android 2.1. Foram introduzidos nessa biblioteca componentes como **Navigation Drawer**, **Floating Action Button**, **Snackbar**, **Tabs** e **Floating labels**. Vamos aprender um por um, começando pelas Tabs.

[Baixe o código aqui]()

## 1. Implementando Tabs

> **Importante**: Vamos usar o *Material Theme* e a *Toolbar* implementada no nosso [Primeiro Post](https://rochadaniel.github.io/android/2016/09/17/Come%C3%A7ando-com-o-Material-Design.html).

**1.** Abra o arquivo **build.gradle** e adicione a biblioteca `com.android.support:design:23.0.1`

```java
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:23.0.1'
    compile 'com.android.support:design:23.0.1'
}
```

**2.** Adicione as seguintes dimensões no arquivo **dimens.xml** que fica dentro de **res ⇒ values**.

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

**3.** Dentro do seu pacote principal crie um Fragment chamado **OneFragment.java** (crie uma classe java normal) e adicione a seguinte linha de código:

```java
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

public class OneFragment extends Fragment{

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
        return inflater.inflate(R.layout.fragment_one, container, false);
    }

}
```

**4.** Abra o arquivo **fragment_one.xml** dentro de **res ⇒ layout** e adicione o seguinte código:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="info.androidhive.materialtabs.fragments.OneFragment">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/one"
        android:textSize="40dp"
        android:textStyle="bold"
        android:layout_centerInParent="true"/>

</RelativeLayout>
```

**5.** Faça o mesmo procedimento de criar o Fragment e seu Layout para quantas tabs forem necessárias.

## 2. Tabs Fixas

Tabs Fixas devem ser usadas quando temos um numero fixo de Tabs.

**6.** Abra o arquivo **activity_main.xml** e adicione o código:

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

**7.** Abra o arquivo **MainActivity.java** e faça as seguintes modificações:

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

import info.androidhive.materialtabs.R;
import info.androidhive.materialtabs.fragments.OneFragment;
import info.androidhive.materialtabs.fragments.ThreeFragment;
import info.androidhive.materialtabs.fragments.TwoFragment;

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

        getSupportActionBar().setDisplayHomeAsUpEnabled(true);

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

--imagem--

## 2.1 Tabs ocupando tela inteira

Se você quer que as suas Tabs ocupem toda a largura da tela, sete `app:tabGravity=”fill"` no seu **TabLayout**.

--imagem--

## 2.2 Tabs centralizadas

Se você quer as suas Tabs centralizadas na tela, sete`app:tabGravity=”center”` no seu **TabLayout**.

--imagem--

## 2.3 Scrollable Tabs

Scrollable Tabs devem ser usadas quando temos um numero grande de tabs, que provavelmente não caberão na tela do aparelho. Para fazer o teste, sete `app:tabMode=”scrollable”` no seu **TabLayout** e adicione em torno de 10 Fragments para poder visualizar o efeito do scroll.

--imagem--

## 3. Tabs com icone e texto

Tudo o que deve ser feito é chamar o método `setIcon()` passando como parametro o icone escolhido.

```
tabLayout.getTabAt(0).setIcon(tabIcons[0]);
tabLayout.getTabAt(1).setIcon(tabIcons[1]);
```

**8.** Abra o arquivo **MainActivity.java** e modifique da seguinte forma. Repare que criamos um vetor de icones `int[] tabIcons` e criamos o método `setupTabIcons()`:

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

public class MainActivity extends AppCompatActivity {

    private Toolbar toolbar;
    private TabLayout tabLayout;
    private ViewPager viewPager;
    private int[] tabIcons = {
            R.drawable.ic_tab_favourite,
            R.drawable.ic_tab_call,
            R.drawable.ic_tab_contacts
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);

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

--imagem--

## 4. Tabs com apenas icones

Modifique o método `getPageTitle()` dentro do nosso adapter (**ViewPagerAdapter**).

```java
@Override
public CharSequence getPageTitle(int position) {

    // return null to display only the icon
    return null;
}
```

--imagem--

## 5. Custom Tab View
