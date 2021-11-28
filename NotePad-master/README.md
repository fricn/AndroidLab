# NotePad期中作业
## 软件工程一班
## 一、基本功能
### 1、时间戳
* 效果演示

![图片](https://user-images.githubusercontent.com/90902968/143773592-1da13c5d-949e-4e52-b164-3baaf1196e13.png)

* 代码演示
```
<!--布局文件-->

<TextView
    android:id="@android:id/text2"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:gravity="center_vertical"
    android:paddingLeft="20dp"
    android:textSize="20dp"
    android:layout_weight="1"/>
```
```
//添加“COLUMN_NAME_MODIFICATION_DATE”

private static final String[] PROJECTION = new String[] {
        NotePad.Notes._ID, // 0
        NotePad.Notes.COLUMN_NAME_TITLE, // 1
        NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,//
};
```
```
//添加适配器内容

String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE,
        NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE} ;
int[] viewIDs = { android.R.id.text1, android.R.id.text2 };
```
```
//获取系统的时间，并进行格式化

ContentValues values = new ContentValues();
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd- HH:mm");
simpleDateFormat.setTimeZone(TimeZone.getTimeZone("GMT+8"));
Long timeNow = Long.valueOf(System.currentTimeMillis());
String sdFormat = simpleDateFormat.format(timeNow);
values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, sdFormat);
```
### 2、搜索
* 效果演示

![图片](https://user-images.githubusercontent.com/90902968/143774157-adf9fa9e-d853-4818-a09d-ae94b1b2a85a.png)

![图片](https://user-images.githubusercontent.com/90902968/143774119-b4a74930-f297-48c2-a2a0-6daad81cdfd6.png)

* 代码演示
```
<!--搜索界面布局文件-->

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <SearchView
        android:iconifiedByDefault="false"
        android:id="@+id/search"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />
    <ListView
        android:id="@+id/list"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />
</LinearLayout>
```
```
//在NoteList中添加搜索界面跳转

case R.id.menu_search_item:
    Intent intent = new Intent(this, NoteSearch.class);
    this.startActivity(intent);
    return true;
```
```
//搜索功能实现界面，NoteSearch

package com.example.android.notepad;
import android.app.Activity;
import android.content.Intent;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.os.Bundle;
import android.widget.ListView;
import android.widget.SearchView;
import android.widget.SimpleCursorAdapter;
import android.widget.TextView;
import android.widget.Toast;

public class NoteSearch extends Activity implements SearchView.OnQueryTextListener
{
    ListView listView;
    SQLiteDatabase sqLiteDatabase;

    private static final String[] PROJECTION = new String[]{
            NotePad.Notes._ID, // 0
            NotePad.Notes.COLUMN_NAME_TITLE, // 1
            NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE
    };

    public boolean onQueryTextSubmit(String query) {
        return false;
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.search_layout);

        SearchView searchView = findViewById(R.id.search);
        Intent intent = getIntent();
        if (intent.getData() == null) {
            intent.setData(NotePad.Notes.CONTENT_URI);
        }
        listView = findViewById(R.id.list);
        sqLiteDatabase = new NotePadProvider.DatabaseHelper(this).getReadableDatabase();

        searchView.setQueryHint("搜索");
        searchView.setOnQueryTextListener(this);
    }
    public boolean onQueryTextChange(String string) {
        String selection1 = NotePad.Notes.COLUMN_NAME_TITLE+" like ? or "+NotePad.Notes.COLUMN_NAME_NOTE+" like ?";
        String[] selection2 = {"%"+string+"%","%"+string+"%"};
        Cursor cursor = sqLiteDatabase.query(
                NotePad.Notes.TABLE_NAME,
                PROJECTION,
                selection1,
                selection2,
                null,
                null,
                NotePad.Notes.DEFAULT_SORT_ORDER
        );
        String[] dataColumns = {
                NotePad.Notes.COLUMN_NAME_TITLE,
                NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE
        } ;

        int[] viewIDs = {android.R.id.text1, android.R.id.text2};
        SimpleCursorAdapter adapter
                = new SimpleCursorAdapter(
                this,
                R.layout.noteslist_item,
                cursor,
                dataColumns,
                viewIDs
        );
        listView.setAdapter(adapter);
        return true;
    }
}
```
## 二、拓展功能
### 1、Setting功能
* 效果演示

![图片](https://user-images.githubusercontent.com/90902968/143774347-9c30bcfd-dc0e-4efd-a899-d2bef258af04.png)

![图片](https://user-images.githubusercontent.com/90902968/143774369-0aae015c-e1ea-4eb6-825f-be7820a237b7.png)

![图片](https://user-images.githubusercontent.com/90902968/143774389-f41ebba4-e0d1-4718-9dd0-78e78650c161.png)

* 代码演示

```
<!--Setting布局界面代码演示-->

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="提醒"
        android:layout_marginLeft="20dp"
        android:layout_marginTop="20dp"
        android:layout_marginBottom="10dp"
        android:textSize="20sp"/>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:background="@drawable/setting"
        android:paddingLeft="30dp"
        android:paddingRight="30dp"
        android:layout_marginLeft="20dp"
        android:layout_marginRight="20dp">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            android:padding="10dp">
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="通知"
                android:layout_weight="1"/>
            <Switch
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:checked="true"
                android:layout_weight="1"/>
        </LinearLayout>
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            android:padding="10dp">
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="默认提醒时间"
                android:layout_weight="1"/>
            <Switch
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:checked="true"
                android:layout_weight="1"/>
        </LinearLayout>
    </LinearLayout>
    ......
</LinearLayout>
```
```
//在NoteList中设置跳转

case R.id.menu_setting:
    Intent intent1 = new Intent(this, NoteSetting.class);
    this.startActivity(intent1);
    return true;
```
```
<!--Setting的drawable文件-->
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <corners android:radius="15dp"/>
    <solid android:color="#79D4EEEF"></solid>
</shape>
```
### 2、更改背景色
点击不同的背景色，会切换笔记背景的颜色
* 效果演示

![图片](https://user-images.githubusercontent.com/90902968/143774647-1a43975e-0d5d-4ea6-bb5a-aa4dc51cba54.png)

![图片](https://user-images.githubusercontent.com/90902968/143774657-107ded87-46f4-4f02-b7be-f158819c09e7.png)

![图片](https://user-images.githubusercontent.com/90902968/143774670-88d85e7c-7c06-448f-9207-6d3ccc677ff9.png)

* 代码演示
```
<!--背景颜色布局界面-->

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:weightSum="0.7">

    <ImageView
        android:layout_width="40dp"
        android:layout_height="60dp"
        android:id="@+id/theme_1"
        android:background="@color/theme_1"
        style="@style/theme_bac"
        android:layout_weight="0.1"
        android:onClick="ColorSelect"
        />
        ......
</LinearLayout>
```
```
//在NoteEditor中添加背景色的选择框，以及选择结果设置

//背景颜色的选择框
    private  void showpopSelectBgWindows(){
        LayoutInflater inflater = LayoutInflater.from(this);
        View view = inflater.inflate(R.layout.theme_bac_color, null);
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle("Theme");//设置标题
        builder.setView(view);
        AlertDialog dialog = builder.create();//获取dialog
        dialog.show();//显示对话框
    }

    //选择并改变背景色
    public void ColorSelect(View view){
        String color;
        View view1 = (View)findViewById(R.id.note);
        switch(view.getId()){
            case R.id.theme_1:
                view1.setBackgroundColor(Color.parseColor("#d4eeef"));
                break;
            case R.id.theme_2:
                view1.setBackgroundColor(Color.parseColor("#fbf0b4"));
                break;
            case R.id.theme_3:
                view1.setBackgroundColor(Color.parseColor("#fde9e8"));
                break;
            case R.id.theme_4:
                view1.setBackgroundColor(Color.parseColor("#C4ECA8"));
                break;
            case R.id.theme_5:
                view1.setBackgroundColor(Color.parseColor("#9CF4EC"));
                break;
            case R.id.theme_6:
                view1.setBackgroundColor(Color.parseColor("#EF7FA5"));
                break;
            case R.id.theme_7:
                view1.setBackgroundColor(Color.parseColor("#AE89DC"));
                break;
        }
```
```
package com.example.android.notepad.application;
import android.app.Application;
import android.content.Context;

import com.example.android.notepad.util.SharedPreferenceUtil;


public class MyApplication extends Application {

    private static Context context;
    private static String bac="#FFFFFF";

    @Override
    public void onCreate() {
        super.onCreate();
        context=getApplicationContext();
        readBackground();
    }

    public static Context getContext() {
        return context;
    }

    public static void setContext(Context context) {
        MyApplication.context = context;
    }

    public static String getBackground() {
        return bac;
    }

    public static void setBackground(String background) {
        MyApplication.bac = background;
    }

    //读取配置文件中的背景颜色
    public static void readBackground(){
        if(SharedPreferenceUtil.getDate("background")==null||SharedPreferenceUtil.getDate("background").equals("")){ }
        else{ bac= SharedPreferenceUtil.getDate("background"); }
    }

   //读取配置文件中的背景颜色
    public static void saveBackground(){
        SharedPreferenceUtil.CommitDate("background",bac);
    }
}
```
### 3、美化设计
* 效果演示

![图片](https://user-images.githubusercontent.com/90902968/143775232-612176c2-f128-444a-ba4b-6156403751c1.png)

* 代码演示
```
<!--笔记列表美化布局界面-->

<LinearLayout android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="5dp"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:background="@drawable/note_text">
        <TextView
            xmlns:android="http://schemas.android.com/apk/res/android"
            android:id="@android:id/text1"
            android:layout_width="match_parent"
            android:layout_height="?android:attr/listPreferredItemHeight"
            android:textAppearance="?android:attr/textAppearanceLarge"
            android:gravity="center_vertical"
            android:paddingLeft="20dp"
            android:singleLine="true"
            android:layout_weight="2"
            />
        <!--    笔记的修改时间-->
        <TextView
            android:id="@android:id/text2"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="center_vertical"
            android:paddingLeft="20dp"
            android:textSize="20dp"
            android:layout_weight="1"/>
    </LinearLayout>
</LinearLayout>
```
```
//在NoteList的onCreat()中设置背景图片

//添加背景图片
Resources res = getResources();
Drawable drawable = res.getDrawable(R.drawable.bac_pic);
this.getWindow().setBackgroundDrawable(drawable);
```
```
<!--笔记列表drawable设置文件(note_text)-->
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="#CCFFFFFF">背景色</solid>
    <corners android:radius="10dp"/>
</shape>
```
## 三、待实现的功能
### 1、笔记排序功能
### 2、笔记文件导出
