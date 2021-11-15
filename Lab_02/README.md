# 实验 2 安卓界面布局
## 一、线性布局
### 关键代码：

![图片](https://user-images.githubusercontent.com/90902968/141696112-2f5b6bb2-32b3-4dde-b9d4-4b87e0b53015.png)
### 运行结果：

<img src="https://user-images.githubusercontent.com/90902968/141695960-62293366-5fee-4c9c-808a-f9f7c40f3ef3.png" width=500>

## 二、约束布局

### 关键代码：

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <EditText
        android:cursorVisible="false"
        android:focusableInTouchMode="true"
        android:focusable="true"
        android:id="@+id/content"
        android:layout_width="410dp"
        android:layout_height="216dp"
        android:layout_marginEnd="8dp"
        android:layout_marginRight="8dp"
        android:gravity="right"
        android:hint="0"
        android:paddingTop="100dp"
        android:textSize="100sp"
        app:layout_constraintBottom_toTopOf="@+id/btn_c"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/btn_c"
        style="@style/ButtonStyle"
        android:layout_width="90dp"
        android:layout_height="90dp"
        android:text="C"
        app:layout_constraintBottom_toTopOf="@+id/btn_num7"
        app:layout_constraintEnd_toStartOf="@+id/btn_div"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/content" />
```

### 运行结果：
![图片](https://user-images.githubusercontent.com/90902968/141735568-88a6ff95-02cf-431f-be14-22a86340c5b5.png)


## 三、表格布局
### 关键代码：

```
<TableRow>
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="     Open..."
                android:minWidth="100dp"
                style="@style/Table_font">
            </TextView>
            <TextView/>
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Ctrl+O"
                android:gravity="end"
                android:minWidth="100dp"
                style="@style/Table_font">
            </TextView>
        </TableRow>
```

### 运行结果：
![图片](https://user-images.githubusercontent.com/90902968/141736977-ce04d875-8e02-4bce-b702-8ce770ab6b99.png)

