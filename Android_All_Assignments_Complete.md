# Assignment 1 - Q1: Send "Hello" from one Activity to another using Intent

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="First Activity"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <Button
        android:id="@+id/btnSend"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Send Hello Message"/>
</LinearLayout>
```

---

## activity_second.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Second Activity"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <TextView
        android:id="@+id/tvMessage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="20sp"
        android:textColor="#FF0000"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.hellointent;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    Button btnSend;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnSend = findViewById(R.id.btnSend);

        btnSend.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(MainActivity.this, SecondActivity.class);
                intent.putExtra("message", "Hello");
                startActivity(intent);
            }
        });
    }
}
```

---

## SecondActivity.java
```java
package com.example.hellointent;

import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class SecondActivity extends AppCompatActivity {

    TextView tvMessage;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        tvMessage = findViewById(R.id.tvMessage);

        String message = getIntent().getStringExtra("message");
        tvMessage.setText("Received: " + message);
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.hellointent">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>

        <activity android:name=".SecondActivity"
            android:exported="false"/>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.hellointent"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```

---

## project-level build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application' version '8.1.0' apply false
}
```
# Assignment 1 - Q2: Activity Life Cycle Demo

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Activity Lifecycle Demo"
        android:textSize="22sp"
        android:textStyle="bold"
        android:layout_marginBottom="16dp"/>

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:id="@+id/tvLog"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textSize="16sp"
            android:text="Lifecycle events will appear here...\n"/>
    </ScrollView>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.lifecycle;

import android.os.Bundle;
import android.util.Log;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    TextView tvLog;
    StringBuilder log = new StringBuilder();
    static final String TAG = "LifecycleDemo";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        tvLog = findViewById(R.id.tvLog);
        addLog("onCreate() called");
    }

    @Override
    protected void onStart() {
        super.onStart();
        addLog("onStart() called");
    }

    @Override
    protected void onResume() {
        super.onResume();
        addLog("onResume() called");
    }

    @Override
    protected void onPause() {
        super.onPause();
        addLog("onPause() called");
    }

    @Override
    protected void onStop() {
        super.onStop();
        addLog("onStop() called");
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        addLog("onRestart() called");
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        addLog("onDestroy() called");
        Log.d(TAG, log.toString());
    }

    private void addLog(String message) {
        log.append(message).append("\n");
        Log.d(TAG, message);
        if (tvLog != null) {
            tvLog.setText(log.toString());
        }
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.lifecycle">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.lifecycle"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
# Assignment 1 - Q3: Read Positive Number and Display Factorial in Another Activity

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Factorial Calculator"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <EditText
        android:id="@+id/etNumber"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter a positive number"
        android:inputType="number"
        android:layout_marginBottom="16dp"/>

    <Button
        android:id="@+id/btnCalculate"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Calculate Factorial"/>
</LinearLayout>
```

---

## activity_result.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Result"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <TextView
        android:id="@+id/tvResult"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="20sp"
        android:textColor="#0000FF"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.factorial;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etNumber;
    Button btnCalculate;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etNumber = findViewById(R.id.etNumber);
        btnCalculate = findViewById(R.id.btnCalculate);

        btnCalculate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String input = etNumber.getText().toString().trim();
                if (input.isEmpty()) {
                    Toast.makeText(MainActivity.this, "Please enter a number", Toast.LENGTH_SHORT).show();
                    return;
                }
                int number = Integer.parseInt(input);
                if (number < 0) {
                    Toast.makeText(MainActivity.this, "Please enter a positive number", Toast.LENGTH_SHORT).show();
                    return;
                }
                Intent intent = new Intent(MainActivity.this, ResultActivity.class);
                intent.putExtra("number", number);
                startActivity(intent);
            }
        });
    }
}
```

---

## ResultActivity.java
```java
package com.example.factorial;

import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class ResultActivity extends AppCompatActivity {

    TextView tvResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_result);

        tvResult = findViewById(R.id.tvResult);

        int number = getIntent().getIntExtra("number", 0);
        long factorial = calculateFactorial(number);
        tvResult.setText("Factorial of " + number + " = " + factorial);
    }

    private long calculateFactorial(int n) {
        if (n == 0 || n == 1) return 1;
        long result = 1;
        for (int i = 2; i <= n; i++) {
            result *= i;
        }
        return result;
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.factorial">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>

        <activity android:name=".ResultActivity"
            android:exported="false"/>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.factorial"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
# Assignment 1 - Q4: Change Color of College Name on Button Click; Change Font Size and Style via XML

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvCollegeName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="My College Name"
        android:textSize="28sp"
        android:textStyle="bold|italic"
        android:textColor="#0000FF"
        android:layout_marginBottom="32dp"/>

    <Button
        android:id="@+id/btnChangeColor"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Change Color"
        android:layout_marginBottom="16dp"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Normal Text - textStyle=normal"
        android:textStyle="normal"
        android:textSize="18sp"
        android:layout_marginBottom="8dp"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Bold Text - textStyle=bold"
        android:textStyle="bold"
        android:textSize="18sp"
        android:layout_marginBottom="8dp"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Italic Text - textStyle=italic"
        android:textStyle="italic"
        android:textSize="18sp"
        android:layout_marginBottom="8dp"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Small Text - textSize=12sp"
        android:textSize="12sp"
        android:layout_marginBottom="8dp"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Large Text - textSize=24sp"
        android:textSize="24sp"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.colorchange;

import android.graphics.Color;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    TextView tvCollegeName;
    Button btnChangeColor;
    int colorIndex = 0;
    int[] colors = {Color.RED, Color.GREEN, Color.BLUE, Color.MAGENTA, Color.CYAN, Color.BLACK};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvCollegeName = findViewById(R.id.tvCollegeName);
        btnChangeColor = findViewById(R.id.btnChangeColor);

        btnChangeColor.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                colorIndex = (colorIndex + 1) % colors.length;
                tvCollegeName.setTextColor(colors[colorIndex]);
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.colorchange">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.colorchange"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
# Assignment 1 - Q5: Arithmetic Operations (ConstraintLayout)

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Arithmetic Operations"
        android:textSize="22sp"
        android:textStyle="bold"
        android:gravity="center"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="16dp"/>

    <EditText
        android:id="@+id/etNum1"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="Enter Number 1"
        android:inputType="numberDecimal"
        app:layout_constraintTop_toBottomOf="@id/tvTitle"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="24dp"/>

    <EditText
        android:id="@+id/etNum2"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="Enter Number 2"
        android:inputType="numberDecimal"
        app:layout_constraintTop_toBottomOf="@id/etNum1"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="12dp"/>

    <Button
        android:id="@+id/btnAdd"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Add"
        app:layout_constraintTop_toBottomOf="@id/etNum2"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginTop="16dp"/>

    <Button
        android:id="@+id/btnSub"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Subtract"
        app:layout_constraintTop_toBottomOf="@id/etNum2"
        app:layout_constraintStart_toEndOf="@id/btnAdd"
        android:layout_marginTop="16dp"
        android:layout_marginStart="8dp"/>

    <Button
        android:id="@+id/btnMul"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Multiply"
        app:layout_constraintTop_toBottomOf="@id/btnAdd"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginTop="8dp"/>

    <Button
        android:id="@+id/btnDiv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Divide"
        app:layout_constraintTop_toBottomOf="@id/btnAdd"
        app:layout_constraintStart_toEndOf="@id/btnMul"
        android:layout_marginTop="8dp"
        android:layout_marginStart="8dp"/>

    <TextView
        android:id="@+id/tvResult"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Result will appear here"
        android:textSize="18sp"
        android:textColor="#0000FF"
        android:gravity="center"
        android:padding="12dp"
        app:layout_constraintTop_toBottomOf="@id/btnMul"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="24dp"/>
</androidx.constraintlayout.widget.ConstraintLayout>
```

---

## MainActivity.java
```java
package com.example.arithmetic;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etNum1, etNum2;
    Button btnAdd, btnSub, btnMul, btnDiv;
    TextView tvResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etNum1 = findViewById(R.id.etNum1);
        etNum2 = findViewById(R.id.etNum2);
        btnAdd = findViewById(R.id.btnAdd);
        btnSub = findViewById(R.id.btnSub);
        btnMul = findViewById(R.id.btnMul);
        btnDiv = findViewById(R.id.btnDiv);
        tvResult = findViewById(R.id.tvResult);

        btnAdd.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (validate()) {
                    double n1 = Double.parseDouble(etNum1.getText().toString());
                    double n2 = Double.parseDouble(etNum2.getText().toString());
                    tvResult.setText("Addition: " + (n1 + n2));
                }
            }
        });

        btnSub.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (validate()) {
                    double n1 = Double.parseDouble(etNum1.getText().toString());
                    double n2 = Double.parseDouble(etNum2.getText().toString());
                    tvResult.setText("Subtraction: " + (n1 - n2));
                }
            }
        });

        btnMul.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (validate()) {
                    double n1 = Double.parseDouble(etNum1.getText().toString());
                    double n2 = Double.parseDouble(etNum2.getText().toString());
                    tvResult.setText("Multiplication: " + (n1 * n2));
                }
            }
        });

        btnDiv.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (validate()) {
                    double n1 = Double.parseDouble(etNum1.getText().toString());
                    double n2 = Double.parseDouble(etNum2.getText().toString());
                    if (n2 == 0) {
                        Toast.makeText(MainActivity.this, "Cannot divide by zero", Toast.LENGTH_SHORT).show();
                    } else {
                        tvResult.setText("Division: " + (n1 / n2));
                    }
                }
            }
        });
    }

    private boolean validate() {
        if (etNum1.getText().toString().trim().isEmpty() || etNum2.getText().toString().trim().isEmpty()) {
            Toast.makeText(this, "Please enter both numbers", Toast.LENGTH_SHORT).show();
            return false;
        }
        return true;
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.arithmetic">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.arithmetic"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
# Assignment 1 - Q6: Accept Two Numbers, Find Power and Average, Display on Next Activity

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Power and Average"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <EditText
        android:id="@+id/etBase"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Base Number"
        android:inputType="numberDecimal"
        android:layout_marginBottom="12dp"/>

    <EditText
        android:id="@+id/etExponent"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Exponent"
        android:inputType="numberDecimal"
        android:layout_marginBottom="24dp"/>

    <Button
        android:id="@+id/btnCalculate"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Calculate"/>
</LinearLayout>
```

---

## activity_result.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Results"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <TextView
        android:id="@+id/tvPower"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:textColor="#FF6600"
        android:layout_marginBottom="16dp"/>

    <TextView
        android:id="@+id/tvAverage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:textColor="#009900"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.poweravg;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etBase, etExponent;
    Button btnCalculate;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etBase = findViewById(R.id.etBase);
        etExponent = findViewById(R.id.etExponent);
        btnCalculate = findViewById(R.id.btnCalculate);

        btnCalculate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String b = etBase.getText().toString().trim();
                String e = etExponent.getText().toString().trim();
                if (b.isEmpty() || e.isEmpty()) {
                    Toast.makeText(MainActivity.this, "Please enter both numbers", Toast.LENGTH_SHORT).show();
                    return;
                }
                double base = Double.parseDouble(b);
                double exponent = Double.parseDouble(e);

                Intent intent = new Intent(MainActivity.this, ResultActivity.class);
                intent.putExtra("base", base);
                intent.putExtra("exponent", exponent);
                startActivity(intent);
            }
        });
    }
}
```

---

## ResultActivity.java
```java
package com.example.poweravg;

import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class ResultActivity extends AppCompatActivity {

    TextView tvPower, tvAverage;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_result);

        tvPower = findViewById(R.id.tvPower);
        tvAverage = findViewById(R.id.tvAverage);

        double base = getIntent().getDoubleExtra("base", 0);
        double exponent = getIntent().getDoubleExtra("exponent", 0);

        double power = Math.pow(base, exponent);
        double average = (base + exponent) / 2;

        tvPower.setText("Power (" + base + "^" + exponent + ") = " + power);
        tvAverage.setText("Average of " + base + " and " + exponent + " = " + average);
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.poweravg">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>

        <activity android:name=".ResultActivity"
            android:exported="false"/>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.poweravg"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
# Assignment 2 - Q1: Student Details Form → Display in Table Format in Another Activity

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Student Details"
            android:textSize="22sp"
            android:textStyle="bold"
            android:layout_marginBottom="16dp"/>

        <EditText
            android:id="@+id/etName"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="First Name"
            android:layout_marginBottom="8dp"/>

        <EditText
            android:id="@+id/etSurname"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Surname"
            android:layout_marginBottom="8dp"/>

        <EditText
            android:id="@+id/etClass"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Class"
            android:layout_marginBottom="8dp"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Gender:"
            android:textSize="16sp"
            android:layout_marginBottom="4dp"/>

        <RadioGroup
            android:id="@+id/rgGender"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            android:layout_marginBottom="8dp">

            <RadioButton
                android:id="@+id/rbMale"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Male"/>

            <RadioButton
                android:id="@+id/rbFemale"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Female"/>
        </RadioGroup>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Hobbies:"
            android:textSize="16sp"
            android:layout_marginBottom="4dp"/>

        <CheckBox
            android:id="@+id/cbReading"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Reading"/>

        <CheckBox
            android:id="@+id/cbSports"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Sports"/>

        <CheckBox
            android:id="@+id/cbMusic"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginBottom="8dp"
            android:text="Music"/>

        <EditText
            android:id="@+id/etMarks"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Marks"
            android:inputType="numberDecimal"
            android:layout_marginBottom="16dp"/>

        <Button
            android:id="@+id/btnSubmit"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Submit"/>
    </LinearLayout>
</ScrollView>
```

---

## activity_display.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <TableLayout
        android:id="@+id/tableLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:stretchColumns="1">

        <TableRow android:background="#3F51B5" android:padding="4dp">
            <TextView android:text="Field" android:textColor="#FFFFFF"
                android:textStyle="bold" android:padding="8dp"/>
            <TextView android:text="Value" android:textColor="#FFFFFF"
                android:textStyle="bold" android:padding="8dp"/>
        </TableRow>

        <TableRow android:background="#E8EAF6">
            <TextView android:text="Name" android:padding="8dp" android:textStyle="bold"/>
            <TextView android:id="@+id/tvName" android:padding="8dp"/>
        </TableRow>

        <TableRow android:background="#FFFFFF">
            <TextView android:text="Surname" android:padding="8dp" android:textStyle="bold"/>
            <TextView android:id="@+id/tvSurname" android:padding="8dp"/>
        </TableRow>

        <TableRow android:background="#E8EAF6">
            <TextView android:text="Class" android:padding="8dp" android:textStyle="bold"/>
            <TextView android:id="@+id/tvClass" android:padding="8dp"/>
        </TableRow>

        <TableRow android:background="#FFFFFF">
            <TextView android:text="Gender" android:padding="8dp" android:textStyle="bold"/>
            <TextView android:id="@+id/tvGender" android:padding="8dp"/>
        </TableRow>

        <TableRow android:background="#E8EAF6">
            <TextView android:text="Hobbies" android:padding="8dp" android:textStyle="bold"/>
            <TextView android:id="@+id/tvHobbies" android:padding="8dp"/>
        </TableRow>

        <TableRow android:background="#FFFFFF">
            <TextView android:text="Marks" android:padding="8dp" android:textStyle="bold"/>
            <TextView android:id="@+id/tvMarks" android:padding="8dp"/>
        </TableRow>
    </TableLayout>
</ScrollView>
```

---

## MainActivity.java
```java
package com.example.studentdetails;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etName, etSurname, etClass, etMarks;
    RadioGroup rgGender;
    RadioButton rbMale, rbFemale;
    CheckBox cbReading, cbSports, cbMusic;
    Button btnSubmit;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etName = findViewById(R.id.etName);
        etSurname = findViewById(R.id.etSurname);
        etClass = findViewById(R.id.etClass);
        etMarks = findViewById(R.id.etMarks);
        rgGender = findViewById(R.id.rgGender);
        rbMale = findViewById(R.id.rbMale);
        rbFemale = findViewById(R.id.rbFemale);
        cbReading = findViewById(R.id.cbReading);
        cbSports = findViewById(R.id.cbSports);
        cbMusic = findViewById(R.id.cbMusic);
        btnSubmit = findViewById(R.id.btnSubmit);

        btnSubmit.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String name = etName.getText().toString().trim();
                String surname = etSurname.getText().toString().trim();
                String cls = etClass.getText().toString().trim();
                String marks = etMarks.getText().toString().trim();

                if (name.isEmpty() || surname.isEmpty() || cls.isEmpty() || marks.isEmpty()) {
                    Toast.makeText(MainActivity.this, "Please fill all fields", Toast.LENGTH_SHORT).show();
                    return;
                }

                String gender = rbMale.isChecked() ? "Male" : rbFemale.isChecked() ? "Female" : "Not selected";

                StringBuilder hobbies = new StringBuilder();
                if (cbReading.isChecked()) hobbies.append("Reading ");
                if (cbSports.isChecked()) hobbies.append("Sports ");
                if (cbMusic.isChecked()) hobbies.append("Music");
                if (hobbies.length() == 0) hobbies.append("None");

                Intent intent = new Intent(MainActivity.this, DisplayActivity.class);
                intent.putExtra("name", name);
                intent.putExtra("surname", surname);
                intent.putExtra("class", cls);
                intent.putExtra("gender", gender);
                intent.putExtra("hobbies", hobbies.toString().trim());
                intent.putExtra("marks", marks);
                startActivity(intent);
            }
        });
    }
}
```

---

## DisplayActivity.java
```java
package com.example.studentdetails;

import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class DisplayActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_display);

        ((TextView) findViewById(R.id.tvName)).setText(getIntent().getStringExtra("name"));
        ((TextView) findViewById(R.id.tvSurname)).setText(getIntent().getStringExtra("surname"));
        ((TextView) findViewById(R.id.tvClass)).setText(getIntent().getStringExtra("class"));
        ((TextView) findViewById(R.id.tvGender)).setText(getIntent().getStringExtra("gender"));
        ((TextView) findViewById(R.id.tvHobbies)).setText(getIntent().getStringExtra("hobbies"));
        ((TextView) findViewById(R.id.tvMarks)).setText(getIntent().getStringExtra("marks"));
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.studentdetails">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>

        <activity android:name=".DisplayActivity"
            android:exported="false"/>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.studentdetails"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
# Assignment 2 - Q2: RadioButton Demo

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Select Your Gender"
        android:textSize="20sp"
        android:textStyle="bold"
        android:layout_marginBottom="16dp"/>

    <RadioGroup
        android:id="@+id/rgGender"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:layout_marginBottom="24dp">

        <RadioButton
            android:id="@+id/rbMale"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Male"
            android:textSize="18sp"/>

        <RadioButton
            android:id="@+id/rbFemale"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Female"
            android:textSize="18sp"/>

        <RadioButton
            android:id="@+id/rbOther"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Other"
            android:textSize="18sp"/>
    </RadioGroup>

    <Button
        android:id="@+id/btnShow"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Show Selection"
        android:layout_marginBottom="16dp"/>

    <TextView
        android:id="@+id/tvResult"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:textColor="#009900"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.radiobutton;

import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    RadioGroup rgGender;
    RadioButton rbMale, rbFemale, rbOther;
    Button btnShow;
    TextView tvResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        rgGender = findViewById(R.id.rgGender);
        rbMale = findViewById(R.id.rbMale);
        rbFemale = findViewById(R.id.rbFemale);
        rbOther = findViewById(R.id.rbOther);
        btnShow = findViewById(R.id.btnShow);
        tvResult = findViewById(R.id.tvResult);

        btnShow.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                int selectedId = rgGender.getCheckedRadioButtonId();
                if (selectedId == -1) {
                    Toast.makeText(MainActivity.this, "Please select a gender", Toast.LENGTH_SHORT).show();
                    return;
                }
                RadioButton selected = findViewById(selectedId);
                String selection = selected.getText().toString();
                tvResult.setText("Selected: " + selection);
                Toast.makeText(MainActivity.this, "You selected: " + selection, Toast.LENGTH_SHORT).show();
            }
        });

        rgGender.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup group, int checkedId) {
                RadioButton rb = findViewById(checkedId);
                if (rb != null) {
                    Toast.makeText(MainActivity.this, "Selected: " + rb.getText(), Toast.LENGTH_SHORT).show();
                }
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.radiobutton">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.radiobutton"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```

---
---

# Assignment 2 - Q3: RadioButtons & CheckBoxes – Display Selected Text Using Toast

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Select Options"
            android:textSize="22sp"
            android:textStyle="bold"
            android:layout_marginBottom="16dp"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Gender:"
            android:textSize="16sp"
            android:textStyle="bold"
            android:layout_marginBottom="4dp"/>

        <RadioGroup
            android:id="@+id/rgGender"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            android:layout_marginBottom="16dp">

            <RadioButton
                android:id="@+id/rbMale"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Male"
                android:layout_marginEnd="16dp"/>

            <RadioButton
                android:id="@+id/rbFemale"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Female"/>
        </RadioGroup>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Hobbies:"
            android:textSize="16sp"
            android:textStyle="bold"
            android:layout_marginBottom="4dp"/>

        <CheckBox
            android:id="@+id/cbCricket"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Cricket"/>

        <CheckBox
            android:id="@+id/cbFootball"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Football"/>

        <CheckBox
            android:id="@+id/cbReading"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Reading"
            android:layout_marginBottom="24dp"/>

        <Button
            android:id="@+id/btnSubmit"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Submit"/>
    </LinearLayout>
</ScrollView>
```

---

## MainActivity.java
```java
package com.example.radiocheckbox;

import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    RadioGroup rgGender;
    RadioButton rbMale, rbFemale;
    CheckBox cbCricket, cbFootball, cbReading;
    Button btnSubmit;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        rgGender = findViewById(R.id.rgGender);
        rbMale = findViewById(R.id.rbMale);
        rbFemale = findViewById(R.id.rbFemale);
        cbCricket = findViewById(R.id.cbCricket);
        cbFootball = findViewById(R.id.cbFootball);
        cbReading = findViewById(R.id.cbReading);
        btnSubmit = findViewById(R.id.btnSubmit);

        btnSubmit.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                StringBuilder sb = new StringBuilder();

                int selectedGenderId = rgGender.getCheckedRadioButtonId();
                if (selectedGenderId != -1) {
                    RadioButton rb = findViewById(selectedGenderId);
                    sb.append("Gender: ").append(rb.getText()).append("\n");
                } else {
                    sb.append("Gender: Not selected\n");
                }

                StringBuilder hobbies = new StringBuilder();
                if (cbCricket.isChecked()) hobbies.append("Cricket ");
                if (cbFootball.isChecked()) hobbies.append("Football ");
                if (cbReading.isChecked()) hobbies.append("Reading");
                sb.append("Hobbies: ").append(hobbies.length() > 0 ? hobbies.toString().trim() : "None");

                Toast.makeText(MainActivity.this, sb.toString(), Toast.LENGTH_LONG).show();
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.radiocheckbox">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.radiocheckbox"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
# Assignment 2 - Q4: Accept Two Numbers, Reject if Both > 10

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Number Validator"
        android:textSize="22sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <EditText
        android:id="@+id/etNum1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Number 1"
        android:inputType="numberDecimal"
        android:layout_marginBottom="12dp"/>

    <EditText
        android:id="@+id/etNum2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Number 2"
        android:inputType="numberDecimal"
        android:layout_marginBottom="24dp"/>

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Submit"
        android:layout_marginBottom="16dp"/>

    <TextView
        android:id="@+id/tvResult"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        android:textColor="#009900"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.numbervalidator;

import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etNum1, etNum2;
    Button btnSubmit;
    TextView tvResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etNum1 = findViewById(R.id.etNum1);
        etNum2 = findViewById(R.id.etNum2);
        btnSubmit = findViewById(R.id.btnSubmit);
        tvResult = findViewById(R.id.tvResult);

        btnSubmit.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String s1 = etNum1.getText().toString().trim();
                String s2 = etNum2.getText().toString().trim();

                if (s1.isEmpty() || s2.isEmpty()) {
                    Toast.makeText(MainActivity.this, "Please enter both numbers", Toast.LENGTH_SHORT).show();
                    return;
                }

                double n1 = Double.parseDouble(s1);
                double n2 = Double.parseDouble(s2);

                if (n1 > 10 && n2 > 10) {
                    Toast.makeText(MainActivity.this, "Both numbers cannot be > 10. Please enter new numbers.", Toast.LENGTH_LONG).show();
                    etNum1.setText("");
                    etNum2.setText("");
                    tvResult.setText("Input rejected! Both numbers > 10");
                    tvResult.setTextColor(0xFFCC0000);
                } else {
                    tvResult.setText("Accepted!\nNumber 1: " + n1 + "\nNumber 2: " + n2);
                    tvResult.setTextColor(0xFF009900);
                }
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.numbervalidator">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.numbervalidator"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```

---
---

# Assignment 2 - Q5: Login Screen with TableLayout (No Database)

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Login"
        android:textSize="28sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <TableLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:stretchColumns="1"
        android:layout_marginBottom="24dp">

        <TableRow android:layout_marginBottom="8dp">
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Username:"
                android:textSize="16sp"
                android:paddingEnd="12dp"
                android:gravity="center_vertical"/>
            <EditText
                android:id="@+id/etUsername"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:hint="Enter username"
                android:inputType="text"/>
        </TableRow>

        <TableRow>
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Password:"
                android:textSize="16sp"
                android:paddingEnd="12dp"
                android:gravity="center_vertical"/>
            <EditText
                android:id="@+id/etPassword"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:hint="Enter password"
                android:inputType="textPassword"/>
        </TableRow>
    </TableLayout>

    <Button
        android:id="@+id/btnLogin"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Login"/>
</LinearLayout>
```

---

## activity_home.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:id="@+id/tvWelcome"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="24sp"
        android:textStyle="bold"
        android:textColor="#009900"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.loginapp;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etUsername, etPassword;
    Button btnLogin;

    // Hardcoded credentials (no database)
    final String VALID_USER = "admin";
    final String VALID_PASS = "1234";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etUsername = findViewById(R.id.etUsername);
        etPassword = findViewById(R.id.etPassword);
        btnLogin = findViewById(R.id.btnLogin);

        btnLogin.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String user = etUsername.getText().toString().trim();
                String pass = etPassword.getText().toString().trim();

                if (user.isEmpty() || pass.isEmpty()) {
                    Toast.makeText(MainActivity.this, "Please enter username and password", Toast.LENGTH_SHORT).show();
                    return;
                }

                if (user.equals(VALID_USER) && pass.equals(VALID_PASS)) {
                    Toast.makeText(MainActivity.this, "Login Successful! Going to next Activity.", Toast.LENGTH_SHORT).show();
                    Intent intent = new Intent(MainActivity.this, HomeActivity.class);
                    intent.putExtra("username", user);
                    startActivity(intent);
                } else {
                    Toast.makeText(MainActivity.this, "Invalid credentials. Try admin/1234", Toast.LENGTH_SHORT).show();
                }
            }
        });
    }
}
```

---

## HomeActivity.java
```java
package com.example.loginapp;

import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class HomeActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_home);

        String username = getIntent().getStringExtra("username");
        TextView tvWelcome = findViewById(R.id.tvWelcome);
        tvWelcome.setText("Welcome, " + username + "!\nLogin Successful.");
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.loginapp">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>

        <activity android:name=".HomeActivity"
            android:exported="false"/>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.loginapp"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```

---
---

# Assignment 2 - Q6: Greeting Application

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Greeting App"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <EditText
        android:id="@+id/etName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter your name"
        android:inputType="textPersonName"
        android:layout_marginBottom="16dp"/>

    <RadioGroup
        android:id="@+id/rgTime"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:layout_marginBottom="16dp">

        <RadioButton android:id="@+id/rbMorning" android:layout_width="wrap_content"
            android:layout_height="wrap_content" android:text="Morning"/>
        <RadioButton android:id="@+id/rbAfternoon" android:layout_width="wrap_content"
            android:layout_height="wrap_content" android:text="Afternoon"/>
        <RadioButton android:id="@+id/rbEvening" android:layout_width="wrap_content"
            android:layout_height="wrap_content" android:text="Evening"/>
    </RadioGroup>

    <Button
        android:id="@+id/btnGreet"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Greet Me!"
        android:layout_marginBottom="24dp"/>

    <TextView
        android:id="@+id/tvGreeting"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="20sp"
        android:textStyle="italic"
        android:textColor="#3F51B5"
        android:gravity="center"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.greeting;

import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etName;
    RadioGroup rgTime;
    RadioButton rbMorning, rbAfternoon, rbEvening;
    Button btnGreet;
    TextView tvGreeting;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etName = findViewById(R.id.etName);
        rgTime = findViewById(R.id.rgTime);
        rbMorning = findViewById(R.id.rbMorning);
        rbAfternoon = findViewById(R.id.rbAfternoon);
        rbEvening = findViewById(R.id.rbEvening);
        btnGreet = findViewById(R.id.btnGreet);
        tvGreeting = findViewById(R.id.tvGreeting);

        btnGreet.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String name = etName.getText().toString().trim();
                if (name.isEmpty()) {
                    Toast.makeText(MainActivity.this, "Please enter your name", Toast.LENGTH_SHORT).show();
                    return;
                }

                String timeOfDay = "Day";
                int selectedId = rgTime.getCheckedRadioButtonId();
                if (selectedId == R.id.rbMorning) timeOfDay = "Morning";
                else if (selectedId == R.id.rbAfternoon) timeOfDay = "Afternoon";
                else if (selectedId == R.id.rbEvening) timeOfDay = "Evening";

                String greeting = "Good " + timeOfDay + ",\n" + name + "!\nWelcome to Android Programming!";
                tvGreeting.setText(greeting);
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.greeting">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.greeting"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
# Assignment 3 - Q1: Factorial with AlertDialog

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Factorial - AlertDialog"
        android:textSize="22sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <EditText
        android:id="@+id/etNumber"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter a number"
        android:inputType="number"
        android:layout_marginBottom="16dp"/>

    <Button
        android:id="@+id/btnCalculate"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Calculate Factorial"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.factorialalert;

import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etNumber;
    Button btnCalculate;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etNumber = findViewById(R.id.etNumber);
        btnCalculate = findViewById(R.id.btnCalculate);

        btnCalculate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String input = etNumber.getText().toString().trim();
                if (input.isEmpty()) {
                    Toast.makeText(MainActivity.this, "Please enter a number", Toast.LENGTH_SHORT).show();
                    return;
                }

                int n = Integer.parseInt(input);
                if (n < 0) {
                    Toast.makeText(MainActivity.this, "Enter a non-negative number", Toast.LENGTH_SHORT).show();
                    return;
                }

                long result = factorial(n);

                new AlertDialog.Builder(MainActivity.this)
                        .setTitle("Factorial Result")
                        .setMessage("Factorial of " + n + " = " + result)
                        .setPositiveButton("OK", null)
                        .show();
            }
        });
    }

    private long factorial(int n) {
        if (n == 0 || n == 1) return 1;
        long result = 1;
        for (int i = 2; i <= n; i++) result *= i;
        return result;
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.factorialalert">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.factorialalert"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```

---
---

# Assignment 3 - Q2: DatePicker and DatePickerDialog Demo

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="DatePicker Demo"
        android:textSize="22sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <DatePicker
        android:id="@+id/datePicker"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:datePickerMode="spinner"
        android:layout_marginBottom="16dp"/>

    <Button
        android:id="@+id/btnGetDate"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Get Date from DatePicker"
        android:layout_marginBottom="8dp"/>

    <Button
        android:id="@+id/btnOpenDialog"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Open DatePickerDialog"
        android:layout_marginBottom="16dp"/>

    <TextView
        android:id="@+id/tvDate"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:textColor="#3F51B5"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.datepicker;

import android.app.DatePickerDialog;
import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;
import java.util.Calendar;

public class MainActivity extends AppCompatActivity {

    DatePicker datePicker;
    Button btnGetDate, btnOpenDialog;
    TextView tvDate;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        datePicker = findViewById(R.id.datePicker);
        btnGetDate = findViewById(R.id.btnGetDate);
        btnOpenDialog = findViewById(R.id.btnOpenDialog);
        tvDate = findViewById(R.id.tvDate);

        btnGetDate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                int day = datePicker.getDayOfMonth();
                int month = datePicker.getMonth() + 1;
                int year = datePicker.getYear();
                tvDate.setText("Selected Date: " + day + "/" + month + "/" + year);
            }
        });

        btnOpenDialog.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Calendar cal = Calendar.getInstance();
                DatePickerDialog dialog = new DatePickerDialog(
                        MainActivity.this,
                        new DatePickerDialog.OnDateSetListener() {
                            @Override
                            public void onDateSet(DatePicker view, int year, int month, int dayOfMonth) {
                                tvDate.setText("Dialog Date: " + dayOfMonth + "/" + (month + 1) + "/" + year);
                            }
                        },
                        cal.get(Calendar.YEAR),
                        cal.get(Calendar.MONTH),
                        cal.get(Calendar.DAY_OF_MONTH)
                );
                dialog.show();
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.datepicker">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.datepicker"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
# Assignment 3 - Q4: ListView with Toast on Item Click

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="ListView Demo"
        android:textSize="22sp"
        android:textStyle="bold"
        android:layout_marginBottom="16dp"/>

    <ListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.listviewdemo;

import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    ListView listView;
    String[] fruits = {"Apple", "Banana", "Cherry", "Date", "Elderberry",
                       "Fig", "Grape", "Honeydew", "Kiwi", "Lemon",
                       "Mango", "Nectarine", "Orange", "Papaya", "Quince"};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        listView = findViewById(R.id.listView);

        ArrayAdapter<String> adapter = new ArrayAdapter<>(
                this,
                android.R.layout.simple_list_item_1,
                fruits
        );

        listView.setAdapter(adapter);

        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                String selectedItem = fruits[position];
                Toast.makeText(MainActivity.this, "Selected: " + selectedItem, Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.listviewdemo">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.listviewdemo"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```

---
---

# Assignment 3 - Q5: TimePicker – Display Selected Time on TextView

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TimePicker Demo"
        android:textSize="22sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <TimePicker
        android:id="@+id/timePicker"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:timePickerMode="spinner"
        android:layout_marginBottom="16dp"/>

    <Button
        android:id="@+id/btnGetTime"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Get Selected Time"
        android:layout_marginBottom="16dp"/>

    <TextView
        android:id="@+id/tvTime"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="20sp"
        android:textStyle="bold"
        android:textColor="#3F51B5"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.timepicker;

import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    TimePicker timePicker;
    Button btnGetTime;
    TextView tvTime;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        timePicker = findViewById(R.id.timePicker);
        btnGetTime = findViewById(R.id.btnGetTime);
        tvTime = findViewById(R.id.tvTime);

        btnGetTime.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                int hour = timePicker.getHour();
                int minute = timePicker.getMinute();

                String amPm = hour >= 12 ? "PM" : "AM";
                int displayHour = hour % 12;
                if (displayHour == 0) displayHour = 12;

                String time = String.format("%02d:%02d %s", displayHour, minute, amPm);
                tvTime.setText("Selected Time: " + time);
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.timepicker">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.timepicker"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
# Assignment 4 - Q1: Google Maps – Satellite View of Current Location

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mapFragment"
    android:name="com.google.android.gms.maps.SupportMapFragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

---

## MainActivity.java
```java
package com.example.googlemapsatellite;

import android.Manifest;
import android.content.pm.PackageManager;
import android.location.Location;
import android.os.Bundle;
import android.widget.Toast;
import androidx.annotation.NonNull;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;
import androidx.fragment.app.FragmentActivity;
import com.google.android.gms.location.FusedLocationProviderClient;
import com.google.android.gms.location.LocationServices;
import com.google.android.gms.maps.CameraUpdateFactory;
import com.google.android.gms.maps.GoogleMap;
import com.google.android.gms.maps.OnMapReadyCallback;
import com.google.android.gms.maps.SupportMapFragment;
import com.google.android.gms.maps.model.LatLng;
import com.google.android.gms.maps.model.MarkerOptions;
import com.google.android.gms.tasks.OnSuccessListener;

public class MainActivity extends FragmentActivity implements OnMapReadyCallback {

    private GoogleMap mMap;
    private FusedLocationProviderClient fusedLocationClient;
    private static final int LOCATION_PERMISSION_REQUEST = 1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        fusedLocationClient = LocationServices.getFusedLocationProviderClient(this);

        SupportMapFragment mapFragment = (SupportMapFragment)
                getSupportFragmentManager().findFragmentById(R.id.mapFragment);
        if (mapFragment != null) {
            mapFragment.getMapAsync(this);
        }
    }

    @Override
    public void onMapReady(@NonNull GoogleMap googleMap) {
        mMap = googleMap;
        mMap.setMapType(GoogleMap.MAP_TYPE_SATELLITE);

        if (ContextCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION)
                == PackageManager.PERMISSION_GRANTED) {
            showCurrentLocation();
        } else {
            ActivityCompat.requestPermissions(this,
                    new String[]{Manifest.permission.ACCESS_FINE_LOCATION},
                    LOCATION_PERMISSION_REQUEST);
        }
    }

    private void showCurrentLocation() {
        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION)
                != PackageManager.PERMISSION_GRANTED) return;

        mMap.setMyLocationEnabled(true);

        fusedLocationClient.getLastLocation().addOnSuccessListener(this, new OnSuccessListener<Location>() {
            @Override
            public void onSuccess(Location location) {
                if (location != null) {
                    LatLng current = new LatLng(location.getLatitude(), location.getLongitude());
                    mMap.addMarker(new MarkerOptions().position(current).title("My Location"));
                    mMap.moveCamera(CameraUpdateFactory.newLatLngZoom(current, 17f));
                } else {
                    // Default location if GPS not available
                    LatLng defaultLoc = new LatLng(19.0760, 72.8777); // Mumbai
                    mMap.addMarker(new MarkerOptions().position(defaultLoc).title("Default Location"));
                    mMap.moveCamera(CameraUpdateFactory.newLatLngZoom(defaultLoc, 15f));
                    Toast.makeText(MainActivity.this, "Location not available, showing Mumbai", Toast.LENGTH_SHORT).show();
                }
            }
        });
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions,
                                           @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == LOCATION_PERMISSION_REQUEST) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                showCurrentLocation();
            } else {
                Toast.makeText(this, "Location permission denied", Toast.LENGTH_SHORT).show();
            }
        }
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.googlemapsatellite">

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    <uses-permission android:name="android.permission.INTERNET"/>

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <!-- REPLACE YOUR_API_KEY with actual Google Maps API Key -->
        <meta-data
            android:name="com.google.android.geo.API_KEY"
            android:value="YOUR_API_KEY"/>

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.googlemapsatellite"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
    implementation 'com.google.android.gms:play-services-maps:18.2.0'
    implementation 'com.google.android.gms:play-services-location:21.0.1'
}
```

---
---

# Assignment 4 - Q7: Google Maps – Zoom In/Out and Hybrid View

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment
        android:id="@+id/mapFragment"
        android:name="com.google.android.gms.maps.SupportMapFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_above="@id/btnBar"/>

    <LinearLayout
        android:id="@+id/btnBar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:layout_alignParentBottom="true"
        android:background="#FFFFFFCC"
        android:padding="8dp">

        <Button
            android:id="@+id/btnZoomIn"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Zoom In"
            android:layout_marginEnd="4dp"/>

        <Button
            android:id="@+id/btnZoomOut"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Zoom Out"
            android:layout_marginEnd="4dp"/>

        <Button
            android:id="@+id/btnHybrid"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Hybrid"/>
    </LinearLayout>
</RelativeLayout>
```

---

## MainActivity.java
```java
package com.example.googlemapcomplete;

import android.Manifest;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import androidx.annotation.NonNull;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;
import androidx.fragment.app.FragmentActivity;
import com.google.android.gms.maps.CameraUpdateFactory;
import com.google.android.gms.maps.GoogleMap;
import com.google.android.gms.maps.OnMapReadyCallback;
import com.google.android.gms.maps.SupportMapFragment;
import com.google.android.gms.maps.model.LatLng;
import com.google.android.gms.maps.model.MarkerOptions;

public class MainActivity extends FragmentActivity implements OnMapReadyCallback {

    private GoogleMap mMap;
    private Button btnZoomIn, btnZoomOut, btnHybrid;
    private static final int LOCATION_PERMISSION_REQUEST = 1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnZoomIn = findViewById(R.id.btnZoomIn);
        btnZoomOut = findViewById(R.id.btnZoomOut);
        btnHybrid = findViewById(R.id.btnHybrid);

        SupportMapFragment mapFragment = (SupportMapFragment)
                getSupportFragmentManager().findFragmentById(R.id.mapFragment);
        if (mapFragment != null) {
            mapFragment.getMapAsync(this);
        }
    }

    @Override
    public void onMapReady(@NonNull GoogleMap googleMap) {
        mMap = googleMap;

        LatLng mumbai = new LatLng(19.0760, 72.8777);
        mMap.addMarker(new MarkerOptions().position(mumbai).title("Mumbai"));
        mMap.moveCamera(CameraUpdateFactory.newLatLngZoom(mumbai, 12f));

        if (ContextCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION)
                == PackageManager.PERMISSION_GRANTED) {
            mMap.setMyLocationEnabled(true);
        } else {
            ActivityCompat.requestPermissions(this,
                    new String[]{Manifest.permission.ACCESS_FINE_LOCATION},
                    LOCATION_PERMISSION_REQUEST);
        }

        btnZoomIn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mMap.animateCamera(CameraUpdateFactory.zoomIn());
            }
        });

        btnZoomOut.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mMap.animateCamera(CameraUpdateFactory.zoomOut());
            }
        });

        btnHybrid.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (mMap.getMapType() == GoogleMap.MAP_TYPE_HYBRID) {
                    mMap.setMapType(GoogleMap.MAP_TYPE_NORMAL);
                    btnHybrid.setText("Hybrid");
                } else {
                    mMap.setMapType(GoogleMap.MAP_TYPE_HYBRID);
                    btnHybrid.setText("Normal");
                }
            }
        });
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions,
                                           @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == LOCATION_PERMISSION_REQUEST && grantResults.length > 0
                && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
            if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION)
                    == PackageManager.PERMISSION_GRANTED) {
                mMap.setMyLocationEnabled(true);
            }
        }
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.googlemapcomplete">

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    <uses-permission android:name="android.permission.INTERNET"/>

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <!-- REPLACE YOUR_API_KEY with actual Google Maps API Key -->
        <meta-data
            android:name="com.google.android.geo.API_KEY"
            android:value="YOUR_API_KEY"/>

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.googlemapcomplete"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
    implementation 'com.google.android.gms:play-services-maps:18.2.0'
    implementation 'com.google.android.gms:play-services-location:21.0.1'
}
```
# Assignment 4 - Q2: SQLite – Customer Table (Insert & Show All)

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Customer Management"
            android:textSize="22sp"
            android:textStyle="bold"
            android:layout_marginBottom="16dp"/>

        <EditText
            android:id="@+id/etId"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Customer ID"
            android:inputType="number"
            android:layout_marginBottom="8dp"/>

        <EditText
            android:id="@+id/etName"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Customer Name"
            android:inputType="text"
            android:layout_marginBottom="8dp"/>

        <EditText
            android:id="@+id/etAddress"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Address"
            android:inputType="text"
            android:layout_marginBottom="8dp"/>

        <EditText
            android:id="@+id/etPhone"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Phone Number"
            android:inputType="phone"
            android:layout_marginBottom="16dp"/>

        <Button
            android:id="@+id/btnInsert"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Insert Customer"
            android:layout_marginBottom="8dp"/>

        <Button
            android:id="@+id/btnShowAll"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Show All Customers (Toast)"/>
    </LinearLayout>
</ScrollView>
```

---

## DatabaseHelper.java
```java
package com.example.customersqlite;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

public class DatabaseHelper extends SQLiteOpenHelper {

    private static final String DB_NAME = "CustomerDB";
    private static final int DB_VERSION = 1;
    private static final String TABLE_NAME = "Customer";

    public DatabaseHelper(Context context) {
        super(context, DB_NAME, null, DB_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String createTable = "CREATE TABLE " + TABLE_NAME +
                " (id INTEGER PRIMARY KEY, name TEXT, address TEXT, phno TEXT)";
        db.execSQL(createTable);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS " + TABLE_NAME);
        onCreate(db);
    }

    public boolean insertCustomer(int id, String name, String address, String phno) {
        SQLiteDatabase db = this.getWritableDatabase();
        ContentValues cv = new ContentValues();
        cv.put("id", id);
        cv.put("name", name);
        cv.put("address", address);
        cv.put("phno", phno);
        long result = db.insert(TABLE_NAME, null, cv);
        return result != -1;
    }

    public String getAllCustomers() {
        SQLiteDatabase db = this.getReadableDatabase();
        Cursor cursor = db.rawQuery("SELECT * FROM " + TABLE_NAME, null);
        StringBuilder sb = new StringBuilder();
        if (cursor.getCount() == 0) {
            sb.append("No records found.");
        } else {
            while (cursor.moveToNext()) {
                sb.append("ID: ").append(cursor.getInt(0))
                  .append(", Name: ").append(cursor.getString(1))
                  .append(", Address: ").append(cursor.getString(2))
                  .append(", Phone: ").append(cursor.getString(3))
                  .append("\n");
            }
        }
        cursor.close();
        return sb.toString();
    }
}
```

---

## MainActivity.java
```java
package com.example.customersqlite;

import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etId, etName, etAddress, etPhone;
    Button btnInsert, btnShowAll;
    DatabaseHelper dbHelper;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etId = findViewById(R.id.etId);
        etName = findViewById(R.id.etName);
        etAddress = findViewById(R.id.etAddress);
        etPhone = findViewById(R.id.etPhone);
        btnInsert = findViewById(R.id.btnInsert);
        btnShowAll = findViewById(R.id.btnShowAll);

        dbHelper = new DatabaseHelper(this);

        btnInsert.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String sid = etId.getText().toString().trim();
                String name = etName.getText().toString().trim();
                String address = etAddress.getText().toString().trim();
                String phone = etPhone.getText().toString().trim();

                if (sid.isEmpty() || name.isEmpty() || address.isEmpty() || phone.isEmpty()) {
                    Toast.makeText(MainActivity.this, "Please fill all fields", Toast.LENGTH_SHORT).show();
                    return;
                }

                int id = Integer.parseInt(sid);
                boolean success = dbHelper.insertCustomer(id, name, address, phone);
                if (success) {
                    Toast.makeText(MainActivity.this, "Customer inserted successfully!", Toast.LENGTH_SHORT).show();
                    etId.setText("");
                    etName.setText("");
                    etAddress.setText("");
                    etPhone.setText("");
                } else {
                    Toast.makeText(MainActivity.this, "Insert failed. ID may already exist.", Toast.LENGTH_SHORT).show();
                }
            }
        });

        btnShowAll.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String data = dbHelper.getAllCustomers();
                Toast.makeText(MainActivity.this, data, Toast.LENGTH_LONG).show();
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.customersqlite">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.customersqlite"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```

---
---

# Assignment 4 - Q3: SQLite – Game Table (Update Badminton Players & Display All)

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Game Database"
            android:textSize="22sp"
            android:textStyle="bold"
            android:layout_marginBottom="16dp"/>

        <EditText
            android:id="@+id/etGno"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Game No"
            android:inputType="number"
            android:layout_marginBottom="8dp"/>

        <EditText
            android:id="@+id/etGname"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Game Name"
            android:inputType="text"
            android:layout_marginBottom="8dp"/>

        <EditText
            android:id="@+id/etType"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Type (Indoor/Outdoor)"
            android:inputType="text"
            android:layout_marginBottom="8dp"/>

        <EditText
            android:id="@+id/etPlayers"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="No. of Players"
            android:inputType="number"
            android:layout_marginBottom="16dp"/>

        <Button
            android:id="@+id/btnInsert"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Insert Game"
            android:layout_marginBottom="8dp"/>

        <Button
            android:id="@+id/btnUpdate"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Update Badminton Players to 4"
            android:layout_marginBottom="8dp"/>

        <Button
            android:id="@+id/btnDisplayAll"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Display All Games"
            android:layout_marginBottom="16dp"/>

        <TextView
            android:id="@+id/tvResult"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textSize="14sp"
            android:padding="8dp"
            android:background="#F5F5F5"/>
    </LinearLayout>
</ScrollView>
```

---

## GameDatabaseHelper.java
```java
package com.example.gamesqlite;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

public class GameDatabaseHelper extends SQLiteOpenHelper {

    private static final String DB_NAME = "GameDB";
    private static final int DB_VERSION = 1;
    private static final String TABLE_NAME = "Game";

    public GameDatabaseHelper(Context context) {
        super(context, DB_NAME, null, DB_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String createTable = "CREATE TABLE " + TABLE_NAME +
                " (gno INTEGER PRIMARY KEY, gname TEXT, type TEXT, no_of_players INTEGER)";
        db.execSQL(createTable);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS " + TABLE_NAME);
        onCreate(db);
    }

    public boolean insertGame(int gno, String gname, String type, int players) {
        SQLiteDatabase db = this.getWritableDatabase();
        ContentValues cv = new ContentValues();
        cv.put("gno", gno);
        cv.put("gname", gname);
        cv.put("type", type);
        cv.put("no_of_players", players);
        long result = db.insert(TABLE_NAME, null, cv);
        return result != -1;
    }

    public int updateBadmintonPlayers() {
        SQLiteDatabase db = this.getWritableDatabase();
        ContentValues cv = new ContentValues();
        cv.put("no_of_players", 4);
        return db.update(TABLE_NAME, cv, "gname = ?", new String[]{"Badminton"});
    }

    public String getAllGames() {
        SQLiteDatabase db = this.getReadableDatabase();
        Cursor cursor = db.rawQuery("SELECT * FROM " + TABLE_NAME, null);
        StringBuilder sb = new StringBuilder();
        if (cursor.getCount() == 0) {
            sb.append("No records found.");
        } else {
            sb.append("Gno | Name | Type | Players\n");
            sb.append("--------------------------------\n");
            while (cursor.moveToNext()) {
                sb.append(cursor.getInt(0)).append(" | ")
                  .append(cursor.getString(1)).append(" | ")
                  .append(cursor.getString(2)).append(" | ")
                  .append(cursor.getInt(3)).append("\n");
            }
        }
        cursor.close();
        return sb.toString();
    }
}
```

---

## MainActivity.java
```java
package com.example.gamesqlite;

import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    EditText etGno, etGname, etType, etPlayers;
    Button btnInsert, btnUpdate, btnDisplayAll;
    TextView tvResult;
    GameDatabaseHelper dbHelper;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etGno = findViewById(R.id.etGno);
        etGname = findViewById(R.id.etGname);
        etType = findViewById(R.id.etType);
        etPlayers = findViewById(R.id.etPlayers);
        btnInsert = findViewById(R.id.btnInsert);
        btnUpdate = findViewById(R.id.btnUpdate);
        btnDisplayAll = findViewById(R.id.btnDisplayAll);
        tvResult = findViewById(R.id.tvResult);

        dbHelper = new GameDatabaseHelper(this);

        btnInsert.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String sgno = etGno.getText().toString().trim();
                String gname = etGname.getText().toString().trim();
                String type = etType.getText().toString().trim();
                String splayers = etPlayers.getText().toString().trim();

                if (sgno.isEmpty() || gname.isEmpty() || type.isEmpty() || splayers.isEmpty()) {
                    Toast.makeText(MainActivity.this, "Fill all fields", Toast.LENGTH_SHORT).show();
                    return;
                }

                boolean ok = dbHelper.insertGame(Integer.parseInt(sgno), gname, type, Integer.parseInt(splayers));
                Toast.makeText(MainActivity.this, ok ? "Game inserted!" : "Insert failed", Toast.LENGTH_SHORT).show();
                if (ok) {
                    etGno.setText(""); etGname.setText(""); etType.setText(""); etPlayers.setText("");
                }
            }
        });

        btnUpdate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                int rows = dbHelper.updateBadmintonPlayers();
                Toast.makeText(MainActivity.this, rows + " row(s) updated for Badminton.", Toast.LENGTH_SHORT).show();
            }
        });

        btnDisplayAll.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                tvResult.setText(dbHelper.getAllGames());
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.gamesqlite">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.gamesqlite"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
# Assignment 4 - Q4: Send Email with Attachment

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Send Email"
        android:textSize="22sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <EditText
        android:id="@+id/etTo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="To (email address)"
        android:inputType="textEmailAddress"
        android:layout_marginBottom="8dp"/>

    <EditText
        android:id="@+id/etSubject"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Subject"
        android:inputType="text"
        android:layout_marginBottom="8dp"/>

    <EditText
        android:id="@+id/etBody"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Message Body"
        android:inputType="textMultiLine"
        android:minLines="3"
        android:layout_marginBottom="16dp"/>

    <Button
        android:id="@+id/btnSendEmail"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Send Email with Attachment"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.sendemail;

import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.os.Environment;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.content.FileProvider;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class MainActivity extends AppCompatActivity {

    EditText etTo, etSubject, etBody;
    Button btnSendEmail;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etTo = findViewById(R.id.etTo);
        etSubject = findViewById(R.id.etSubject);
        etBody = findViewById(R.id.etBody);
        btnSendEmail = findViewById(R.id.btnSendEmail);

        btnSendEmail.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String to = etTo.getText().toString().trim();
                String subject = etSubject.getText().toString().trim();
                String body = etBody.getText().toString().trim();

                if (to.isEmpty()) {
                    Toast.makeText(MainActivity.this, "Please enter recipient email", Toast.LENGTH_SHORT).show();
                    return;
                }

                // Create a dummy attachment file
                File attachmentFile = createAttachmentFile();

                Intent emailIntent = new Intent(Intent.ACTION_SEND);
                emailIntent.setType("message/rfc822");
                emailIntent.putExtra(Intent.EXTRA_EMAIL, new String[]{to});
                emailIntent.putExtra(Intent.EXTRA_SUBJECT, subject);
                emailIntent.putExtra(Intent.EXTRA_TEXT, body);

                if (attachmentFile != null) {
                    Uri attachmentUri = FileProvider.getUriForFile(
                            MainActivity.this,
                            getPackageName() + ".provider",
                            attachmentFile
                    );
                    emailIntent.putExtra(Intent.EXTRA_STREAM, attachmentUri);
                    emailIntent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
                }

                try {
                    startActivity(Intent.createChooser(emailIntent, "Send Email via..."));
                } catch (Exception e) {
                    Toast.makeText(MainActivity.this, "No email app found", Toast.LENGTH_SHORT).show();
                }
            }
        });
    }

    private File createAttachmentFile() {
        try {
            File dir = new File(getFilesDir(), "attachments");
            if (!dir.exists()) dir.mkdirs();
            File file = new File(dir, "attachment.txt");
            FileWriter writer = new FileWriter(file);
            writer.write("This is the attachment content.\nSent from Android App.");
            writer.close();
            return file;
        } catch (IOException e) {
            e.printStackTrace();
            return null;
        }
    }
}
```

---

## res/xml/file_paths.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<paths>
    <files-path name="attachments" path="attachments/"/>
</paths>
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.sendemail">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>

        <provider
            android:name="androidx.core.content.FileProvider"
            android:authorities="${applicationId}.provider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths"/>
        </provider>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.sendemail"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```

---
---

# Assignment 4 - Q5: Switch and ToggleButton Demo

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Switch &amp; ToggleButton Demo"
        android:textSize="22sp"
        android:textStyle="bold"
        android:layout_marginBottom="32dp"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Switch:"
        android:textSize="18sp"
        android:layout_marginBottom="8dp"/>

    <Switch
        android:id="@+id/switchDemo"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="WiFi"
        android:textSize="16sp"
        android:layout_marginBottom="16dp"/>

    <TextView
        android:id="@+id/tvSwitchStatus"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        android:textColor="#CC0000"
        android:layout_marginBottom="32dp"
        android:text="Switch is OFF"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="ToggleButton:"
        android:textSize="18sp"
        android:layout_marginBottom="8dp"/>

    <ToggleButton
        android:id="@+id/toggleButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textOn="Bluetooth ON"
        android:textOff="Bluetooth OFF"
        android:layout_marginBottom="16dp"/>

    <TextView
        android:id="@+id/tvToggleStatus"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        android:textColor="#CC0000"
        android:text="Bluetooth is OFF"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.switchtoggle;

import android.os.Bundle;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    Switch switchDemo;
    ToggleButton toggleButton;
    TextView tvSwitchStatus, tvToggleStatus;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        switchDemo = findViewById(R.id.switchDemo);
        toggleButton = findViewById(R.id.toggleButton);
        tvSwitchStatus = findViewById(R.id.tvSwitchStatus);
        tvToggleStatus = findViewById(R.id.tvToggleStatus);

        switchDemo.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                if (isChecked) {
                    tvSwitchStatus.setText("Switch is ON (WiFi Enabled)");
                    tvSwitchStatus.setTextColor(0xFF009900);
                } else {
                    tvSwitchStatus.setText("Switch is OFF (WiFi Disabled)");
                    tvSwitchStatus.setTextColor(0xFFCC0000);
                }
                Toast.makeText(MainActivity.this, "WiFi " + (isChecked ? "ON" : "OFF"), Toast.LENGTH_SHORT).show();
            }
        });

        toggleButton.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                if (isChecked) {
                    tvToggleStatus.setText("Bluetooth is ON");
                    tvToggleStatus.setTextColor(0xFF009900);
                } else {
                    tvToggleStatus.setText("Bluetooth is OFF");
                    tvToggleStatus.setTextColor(0xFFCC0000);
                }
                Toast.makeText(MainActivity.this, "Bluetooth " + (isChecked ? "ON" : "OFF"), Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.switchtoggle">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.switchtoggle"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```

---
---

# Assignment 4 - Q6: RatingBar – Display Stars on Toast and TextView

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Rate Us!"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <RatingBar
        android:id="@+id/ratingBar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:numStars="5"
        android:stepSize="1.0"
        android:rating="0"
        android:layout_marginBottom="24dp"/>

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Submit Rating"
        android:layout_marginBottom="16dp"/>

    <TextView
        android:id="@+id/tvRating"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:textColor="#FF9800"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.ratingbar;

import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    RatingBar ratingBar;
    Button btnSubmit;
    TextView tvRating;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ratingBar = findViewById(R.id.ratingBar);
        btnSubmit = findViewById(R.id.btnSubmit);
        tvRating = findViewById(R.id.tvRating);

        ratingBar.setOnRatingBarChangeListener(new RatingBar.OnRatingBarChangeListener() {
            @Override
            public void onRatingChanged(RatingBar ratingBar, float rating, boolean fromUser) {
                tvRating.setText("You selected: " + (int) rating + " star(s)");
            }
        });

        btnSubmit.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                int stars = (int) ratingBar.getRating();
                String msg = "You rated: " + stars + " out of 5 stars!";
                Toast.makeText(MainActivity.this, msg, Toast.LENGTH_LONG).show();
                tvRating.setText(msg);
            }
        });
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.ratingbar">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.ratingbar"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```

---
---

# Assignment 4 - Q8: Send SMS

---

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Send SMS"
        android:textSize="22sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp"/>

    <EditText
        android:id="@+id/etPhone"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Phone Number"
        android:inputType="phone"
        android:layout_marginBottom="12dp"/>

    <EditText
        android:id="@+id/etMessage"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Message"
        android:inputType="textMultiLine"
        android:minLines="3"
        android:layout_marginBottom="24dp"/>

    <Button
        android:id="@+id/btnSendSMS"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Send SMS"/>
</LinearLayout>
```

---

## MainActivity.java
```java
package com.example.sendsms;

import android.Manifest;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.telephony.SmsManager;
import android.view.View;
import android.widget.*;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

public class MainActivity extends AppCompatActivity {

    EditText etPhone, etMessage;
    Button btnSendSMS;
    private static final int SMS_PERMISSION_CODE = 100;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etPhone = findViewById(R.id.etPhone);
        etMessage = findViewById(R.id.etMessage);
        btnSendSMS = findViewById(R.id.btnSendSMS);

        btnSendSMS.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String phone = etPhone.getText().toString().trim();
                String message = etMessage.getText().toString().trim();

                if (phone.isEmpty() || message.isEmpty()) {
                    Toast.makeText(MainActivity.this, "Please enter phone and message", Toast.LENGTH_SHORT).show();
                    return;
                }

                if (ContextCompat.checkSelfPermission(MainActivity.this, Manifest.permission.SEND_SMS)
                        == PackageManager.PERMISSION_GRANTED) {
                    sendSMS(phone, message);
                } else {
                    ActivityCompat.requestPermissions(MainActivity.this,
                            new String[]{Manifest.permission.SEND_SMS}, SMS_PERMISSION_CODE);
                }
            }
        });
    }

    private void sendSMS(String phone, String message) {
        try {
            SmsManager smsManager = SmsManager.getDefault();
            smsManager.sendTextMessage(phone, null, message, null, null);
            Toast.makeText(this, "SMS sent successfully!", Toast.LENGTH_SHORT).show();
            etPhone.setText("");
            etMessage.setText("");
        } catch (Exception e) {
            Toast.makeText(this, "SMS failed: " + e.getMessage(), Toast.LENGTH_LONG).show();
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions,
                                           @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == SMS_PERMISSION_CODE) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                sendSMS(etPhone.getText().toString(), etMessage.getText().toString());
            } else {
                Toast.makeText(this, "SMS permission denied", Toast.LENGTH_SHORT).show();
            }
        }
    }
}
```

---

## AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.sendsms">

    <uses-permission android:name="android.permission.SEND_SMS"/>

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

---

## app/build.gradle (Groovy DSL)
```groovy
plugins {
    id 'com.android.application'
}

android {
    compileSdk 34

    defaultConfig {
        applicationId "com.example.sendsms"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.10.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```
