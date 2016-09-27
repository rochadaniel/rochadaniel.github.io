---
layout: post
title:  "Animando FAB na troca de tabs"
date:   2016-09-25 12:00:00 +0000
categories: [android]
---

Nesse post estarei mostrando como fazer uma animação de movimento do `Floating Action Button` durante a transição de duas `Tabs`.

> Caso queira conhecer as Tabs mais afundo, confira [esse post](https://rochadaniel.github.io/android/2016/09/22/Android-Trabalhando-com-Tabs.html).

[Baixe o código completo aqui](https://github.com/rochadaniel/FloatingActionButtonViewPagerAnimation)

## 1. Implementando Tabs

**1.** Abra o arquivo **styles.xml** dentro de **res ⇒ values** e adicione as seguintes linhas de código. Os estilos definidos em **styles.xml** são comuns para todas as versões do Android. Vamos chamar nosso estilo de **MyTheme**.

```xml
<resources>

  <style name="MyTheme" parent="MyTheme.Base">

  </style>

  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
      <item name="windowNoTitle">true</item>
      <item name="windowActionBar">false</item>
      <item name="colorPrimary">@color/colorPrimary</item>
      <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
      <item name="colorAccent">@color/colorAccent</item>
  </style>

</resources>
```

**2.** Também dentro de **res ⇒ values**, abra o arquivo (ou crie se não existir) **dimens.xml** e adicione o seguinte código:

```xml
<resources>
    <!-- Default screen margins, per the Android Design guidelines. -->
    <dimen name="activity_horizontal_margin">16dp</dimen>
    <dimen name="activity_vertical_margin">16dp</dimen>
    <dimen name="fab_compat_margin">0dp</dimen>
</resources>
```

**3.** Agora dentro de **res**, crie uma pasta chamada **values-v21**. Dentro dela, crie outro arquivo **styles.xml** com os seguintes estilos. Esses estilos são especificos para versões superiores ao **Android Lollipop**.

```xml
<resources>

  <style name="MyTheme" parent="MyTheme.Base">
      <item name="android:windowContentTransitions">true</item>
      <item name="android:windowAllowEnterTransitionOverlap">true</item>
      <item name="android:windowAllowReturnTransitionOverlap">true</item>
      <item name="android:windowSharedElementEnterTransition">@android:transition/move</item>
      <item name="android:windowSharedElementExitTransition">@android:transition/move</item>
  </style>

</resources>
```

**4.** Também dentro de **res ⇒ values-v21**, abra o arquivo (ou crie se não existir) **dimens.xml** e adicione o seguinte código:

```xml
<resources>
    <dimen name="fab_compat_margin">16dp</dimen>
</resources>
```

> Nas versões do Lollipop para cima, é necessário adicionar uma margin de 16dp no FAB para seguir o guideline do Material Design

**5.** Para aplicar o tema no nosso aplicativo, vamos abrir o arquivo **AndroidManifest.xml** e modificar o atributo **android:theme** que fica dentro da tag **application**.

```xml
android:theme="@style/MyTheme"
```

Depois de aplicar o tema, seu arquivo **AndroidManifest.xml** vai ficar parecido com:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="danielrocha.floatingbuttonanimation">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/MyTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

**6.** Abra o arquivo **build.gradle** e adicione a biblioteca `om.android.support:design:24.2.0`

```java
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:24.2.0'
    compile 'com.android.support:design:24.2.0'
}
```

> Estou utlizando a versão `24.2.0` porque atualizei o Android Studio, mas não tem problema utilizar as versões anteriores como a `23.4.0` por exemplo


Antes de começarmos a criar nossas **Tabs**, vamos criar nossos **Fragments**. Cada um desses Fragments será responsável pela UI de cada Tab.

**7.** Dentro do seu pacote principal crie um Fragment chamado **HomeFragment.java** (crie uma classe java normal) e adicione a seguinte linha de código:

```java
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

/**
 * Created by danielrocha on 23/09/16.
 */

public class HomeFragment extends Fragment {

    public HomeFragment() {
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
        View view = inflater.inflate(R.layout.fragment_layout, container, false);

        ((TextView) view.findViewById(R.id.fragmentText)).setText("One");

        return view;
    }

}
```

**8.** Abra o arquivo **fragment_layout.xml** dentro de **res ⇒ layout** e adicione o seguinte código:

```xml
<<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:tools="http://schemas.android.com/tools"
    tools:context="danielrocha.floatingbuttonanimation.HomeFragment">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/fragmentText"
        android:textSize="40dp"
        android:textStyle="bold"
        android:layout_centerInParent="true"/>

</RelativeLayout>
```

**9.** Agora vamos criar nosso outro Fragment chamado **MenuFragment.java**:

```java
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

/**
 * Created by danielrocha on 23/09/16.
 */

public class MenuFragment extends Fragment {

    public MenuFragment() {
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
        View view = inflater.inflate(R.layout.fragment_layout, container, false);

        ((TextView) view.findViewById(R.id.fragmentText)).setText("Two");

        return view;
    }

}
```

> Utilizamos um unico layout para todos os fragments, mudando apenas o texto de acordo com a sua posição mas é possivel que utilize diferente layouts para cada fragment.

**10.** Abra o arquivo **activity_main.xml** e adicione o seguinte código:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@android:color/white">


    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:minHeight="?attr/actionBarSize"
        app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
        app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
        android:background="@color/colorPrimary">


    </android.support.v7.widget.Toolbar>

    <android.support.design.widget.TabLayout
        android:id="@+id/tabs"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:tabMode="fixed"
        app:tabGravity="fill"
        app:tabTextColor="@android:color/white"
        app:tabSelectedTextColor="@android:color/white"
        android:layout_below="@+id/toolbar"
        android:background="@color/colorPrimary"/>

    <android.support.v4.view.ViewPager
        android:id="@+id/viewpager"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"
        android:layout_below="@+id/tabs"/>

    <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_share_white_24dp"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true"
        android:layout_margin="@dimen/fab_compat_margin"
        app:fabSize="normal"
        android:visibility="visible"/>

</RelativeLayout>
```

> **Baixe os icones necessários para o projeto:** [Icone de compartilhamento](https://design.google.com/icons/#ic_share) e [Icone de confirmação](https://design.google.com/icons/#ic_done)

**11.** Dentro do seu pacote principal crie uma classe chamada **ViewPagerAdapter.java** e adicione a seguinte linha de código:

```java
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentPagerAdapter;
import android.util.SparseArray;
import android.view.ViewGroup;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by danielrocha on 23/09/16.
 */

public class ViewPagerAdapter extends FragmentPagerAdapter {
    private final List<Fragment> mFragmentList = new ArrayList<>();
    private final List<String> mFragmentTitleList = new ArrayList<>();
    SparseArray<Fragment> registeredFragments = new SparseArray<Fragment>();

    public ViewPagerAdapter(FragmentManager manager) {
        super(manager);
    }

    @Override
    public Object instantiateItem(ViewGroup container, int position) {
        Fragment fragment = (Fragment) super.instantiateItem(container, position);
        registeredFragments.put(position, fragment);
        return fragment;
    }

    @Override
    public void destroyItem(ViewGroup container, int position, Object object) {
        registeredFragments.remove(position);
        super.destroyItem(container, position, object);
    }

    public Fragment getRegisteredFragment(int position) {
        return registeredFragments.get(position);
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
```

**12.** Abra o arquivo **MainActivity.java** e adicione a seguinte linha de código:

```java
import android.content.Context;
import android.os.Build;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.TabLayout;
import android.support.v4.content.ContextCompat;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.support.v7.widget.Toolbar;
import android.util.Log;
import android.view.View;
import android.view.animation.Interpolator;
import android.widget.LinearLayout;
import android.widget.RelativeLayout;
import android.widget.Scroller;
import android.widget.Toast;

import java.lang.reflect.Field;

public class MainActivity extends AppCompatActivity {

    private Toolbar toolbar;
    private ViewPager viewPager;
    private TabLayout tabLayout;
    private FloatingActionButton fab;
    private ViewPagerAdapter adapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        toolbar = (Toolbar) findViewById(R.id.toolbar);
        viewPager = (ViewPager) findViewById(R.id.viewpager);
        tabLayout = (TabLayout) findViewById(R.id.tabs);
        fab = (FloatingActionButton) findViewById(R.id.fab);

        setSupportActionBar(toolbar);
        getSupportActionBar().setTitle("FloatingActionButton Animation");

        initViewPager();
        initTabLayout();

    }

    @Override
    public void onBackPressed()
    {
        if(viewPager != null && viewPager.getCurrentItem() == 1) {
            viewPager.setCurrentItem(0);
        } else {
            super.onBackPressed();
        }
    }

    private void initViewPager() {
        adapter = new ViewPagerAdapter(getSupportFragmentManager());
        adapter.addFragment(new HomeFragment(), "HOME");
        adapter.addFragment(new MenuFragment(), "MENU");
        viewPager.setOffscreenPageLimit(2);
        viewPager.setAdapter(adapter);

        changePagerScroller(viewPager);

        viewPager.setCurrentItem(0);
    }

    private void initTabLayout() {
        tabLayout.setupWithViewPager(viewPager);
    }

    public void changePagerScroller(ViewPager mViewPager) {
        try {
            Field mScroller = null;
            mScroller = ViewPager.class.getDeclaredField("mScroller");
            mScroller.setAccessible(true);
            ViewPagerScroller scroller = new ViewPagerScroller(mViewPager.getContext());
            mScroller.set(mViewPager, scroller);
        } catch (Exception e) {
            Log.e("SCROLL", "error of change scroller ", e);
        }
    }

    public class ViewPagerScroller extends Scroller {
        private int mScrollDuration = 600;
        public ViewPagerScroller(Context context) {
            super(context);
        }
        public ViewPagerScroller(Context context, Interpolator interpolator) {
            super(context, interpolator);
        }
        @Override
        public void startScroll(int startX, int startY, int dx, int dy, int duration) {
            super.startScroll(startX, startY, dx, dy, mScrollDuration);
        }
        @Override
        public void startScroll(int startX, int startY, int dx, int dy) {
            super.startScroll(startX, startY, dx, dy, mScrollDuration);
        }
    }

}
```

Rode o app e você verá algo parecido com a imagem abaixo:

![](https://media.giphy.com/media/l2SpN0L9Ju8knU5Ec/giphy.gif)

Note que não temos nenhuma animação do `FAB`, ele continua estático quando mudamos de `Tab`.

## 2. Implementando Animação

Agora vamos para a implementação da animação.

**13.** Dentro da **MainActivity.java** vamos fazer as seguintes alterações:

Inclua o atributo `private int viewPagerWidth;`, ele será responsável por pegar a **largura do nosso ViewPager** e caso a versão do aparelho seja maior que a **KitKat**, teremos que diminuir essa largura pela margem que selecionamos no nosso arquivo **dimens.xml**. Caso a posição do ViewPager seja a primeira (0) iremos usar a animação de **Translation** para movimentar do centro até o final da tela. Iremos adicionar essa lógica no evento de mudança de tabs, chamado `addOnPageChangeListener`, observe no código abaixo:

```java
viewPager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {

            @Override
            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
                final float scale = getResources().getDisplayMetrics().density;
                viewPagerWidth = viewPager.getWidth();
                if(Build.VERSION.SDK_INT > Build.VERSION_CODES.KITKAT) {
                    int dp = (int) (getResources().getDimension(R.dimen.fab_compat_margin) * scale);
                    viewPagerWidth = viewPager.getWidth() - dp;
                }
                if(position == 0) {
                    // Transition from page 0 to page 1 (horizontal shift)
                    int translationFabX = (int) (((viewPagerWidth - fab.getWidth()) / 2f) * positionOffset);
                    fab.setTranslationX(translationFabX);
                    fab.setTranslationY(0);
                }
            }

            @Override
            public void onPageSelected(int position) {

            }

            @Override
            public void onPageScrollStateChanged(int state) {

            }
        });
```

Adicione esse trecho de código no seu método `initViewPager()` e rode o app, você deverá ver seu app da seguinte forma:

![](https://media.giphy.com/media/l2SpNaUlNGrLFSUTe/giphy.gif)

## 2. Alterando icone do FAB de acordo com a Tab

Iremos alterar o icone do nosso `FAB` quando mudarmos de `Tab`. Para isso, iremos implementar dentro do método `onPageSelected` do evento `addOnPageChangeListener` do nosso **ViewPager**.

**14.** Adicione o trecho de código dentro do método `onPageSelected`:

```java
@Override
  public void onPageSelected(int position) {

      switch (position) {
          case 0:
              fab.setImageDrawable(ContextCompat.getDrawable(MainActivity.this, R.drawable.ic_share_white_24dp));
              break;
          case 1:
              fab.setImageDrawable(ContextCompat.getDrawable(MainActivity.this, R.drawable.ic_done_white_24dp));
              break;
      }

  }
```

Sendo assim, você deverá rodar seu app e visualizar da seguinte forma:

![](https://media.giphy.com/media/26hisQNKfRoxLmed2/giphy.gif)

## 3. Implementando click no FAB

Vamos implementar **clicks** diferentes para o nosso `FAB` de acordo com a posição do **ViewPager**.

**15.** Adicione o seguinte trecho de código dentro do método `onCreate` da nossa **MainActivity.java**:

```java
fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                if(viewPager.getCurrentItem() == 0) {
                    Toast.makeText(MainActivity.this, "Home", Toast.LENGTH_SHORT).show();
                } else {
                    Toast.makeText(MainActivity.this, "Menu", Toast.LENGTH_SHORT).show();
                }
            }
        });
```

Sendo assim nossa **MainActivity.java** ficará parecida com:

```java
import android.content.Context;
import android.os.Build;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.TabLayout;
import android.support.v4.content.ContextCompat;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.support.v7.widget.Toolbar;
import android.util.Log;
import android.view.View;
import android.view.animation.Interpolator;
import android.widget.LinearLayout;
import android.widget.RelativeLayout;
import android.widget.Scroller;
import android.widget.Toast;

import java.lang.reflect.Field;

public class MainActivity extends AppCompatActivity {

    private Toolbar toolbar;
    private ViewPager viewPager;
    private TabLayout tabLayout;
    private FloatingActionButton fab;
    private ViewPagerAdapter adapter;
    private int viewPagerWidth;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        toolbar = (Toolbar) findViewById(R.id.toolbar);
        viewPager = (ViewPager) findViewById(R.id.viewpager);
        tabLayout = (TabLayout) findViewById(R.id.tabs);
        fab = (FloatingActionButton) findViewById(R.id.fab);

        setSupportActionBar(toolbar);
        getSupportActionBar().setTitle("FloatingActionButton Animation");

        initViewPager();
        initTabLayout();

        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                if(viewPager.getCurrentItem() == 0) {
                    Toast.makeText(MainActivity.this, "Home", Toast.LENGTH_SHORT).show();
                } else {
                    Toast.makeText(MainActivity.this, "Menu", Toast.LENGTH_SHORT).show();
                }
            }
        });

    }

    @Override
    public void onBackPressed()
    {
        if(viewPager != null && viewPager.getCurrentItem() == 1) {
            viewPager.setCurrentItem(0);
        } else {
            super.onBackPressed();
        }
    }

    private void initViewPager() {
        adapter = new ViewPagerAdapter(getSupportFragmentManager());
        adapter.addFragment(new HomeFragment(), "HOME");
        adapter.addFragment(new MenuFragment(), "MENU");
        viewPager.setOffscreenPageLimit(2);
        viewPager.setAdapter(adapter);
        viewPager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {

            @Override
            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
                final float scale = getResources().getDisplayMetrics().density;
                viewPagerWidth = viewPager.getWidth();
                if(Build.VERSION.SDK_INT > Build.VERSION_CODES.KITKAT) {
                    int dp = (int) (getResources().getDimension(R.dimen.fab_compat_margin) * scale);
                    viewPagerWidth = viewPager.getWidth() - dp;
                }
                if(position == 0) {
                    // Transition from page 0 to page 1 (horizontal shift)
                    int translationFabX = (int) (((viewPagerWidth - fab.getWidth()) / 2f) * positionOffset);
                    fab.setTranslationX(translationFabX);
                    fab.setTranslationY(0);
                }
            }

            @Override
            public void onPageSelected(int position) {

                switch (position) {
                    case 0:
                        fab.setImageDrawable(ContextCompat.getDrawable(MainActivity.this, R.drawable.ic_share_white_24dp));
                        break;
                    case 1:
                        fab.setImageDrawable(ContextCompat.getDrawable(MainActivity.this, R.drawable.ic_done_white_24dp));
                        break;
                }

            }

            @Override
            public void onPageScrollStateChanged(int state) {

            }
        });

        changePagerScroller(viewPager);

        viewPager.setCurrentItem(0);
    }

    private void initTabLayout() {
        tabLayout.setupWithViewPager(viewPager);
    }

    public void changePagerScroller(ViewPager mViewPager) {
        try {
            Field mScroller = null;
            mScroller = ViewPager.class.getDeclaredField("mScroller");
            mScroller.setAccessible(true);
            ViewPagerScroller scroller = new ViewPagerScroller(mViewPager.getContext());
            mScroller.set(mViewPager, scroller);
        } catch (Exception e) {
            Log.e("SCROLL", "error of change scroller ", e);
        }
    }

    public class ViewPagerScroller extends Scroller {
        private int mScrollDuration = 600;
        public ViewPagerScroller(Context context) {
            super(context);
        }
        public ViewPagerScroller(Context context, Interpolator interpolator) {
            super(context, interpolator);
        }
        @Override
        public void startScroll(int startX, int startY, int dx, int dy, int duration) {
            super.startScroll(startX, startY, dx, dy, mScrollDuration);
        }
        @Override
        public void startScroll(int startX, int startY, int dx, int dy) {
            super.startScroll(startX, startY, dx, dy, mScrollDuration);
        }
    }

}
```

Ao rodar o app, veremos da seguinte forma:

![](https://media.giphy.com/media/3o7TKPpqQNsO3a0hRC/giphy.gif)

Obrigado. Para qualquer dúvida ou sugestão, não deixe de comentar :)

[Baixe o código completo aqui](https://github.com/rochadaniel/FloatingActionButtonViewPagerAnimation)
