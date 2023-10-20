# AD-External_Storage
## Permission
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
</manifest>
```

## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    >

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="username"
        android:id="@+id/t1"
        android:padding="40px"

        />
    <EditText

        android:layout_width="300px"
        android:layout_height="wrap_content"
        android:id="@+id/e1"
        android:layout_toRightOf="@+id/t1"

        />
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="password"
        android:layout_below="@+id/t1"
        android:id="@+id/t2"
        android:padding="40px"
        />
    <EditText
        android:layout_width="300px"
        android:layout_height="wrap_content"
        android:id="@+id/e2"
        android:layout_below="@+id/e1"
        android:layout_toRightOf="@+id/t2"
        />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/b"
        android:layout_below="@+id/t2"
        android:text="Login"
        />

</RelativeLayout>
```
## MainActivity.java
```java

package com.example.external_storage;

import androidx.appcompat.app.AppCompatActivity;
import android.content.Intent;
import android.os.Bundle;
import android.os.Environment;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import java.io.File;
import java.io.FileOutputStream;

public class MainActivity extends AppCompatActivity {
    private EditText e1, e2;
    private Button b;
    private Intent intent;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        e1 = findViewById(R.id.e1);
        e2 = findViewById(R.id.e2);
        b = findViewById(R.id.b);

        b.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                String username = e1.getText().toString() + "\n";
                String password = e2.getText().toString();


                try {
                    String fileName = "user_credentials.txt";
                    String filepath = "files";
                    File file = new File(getExternalFilesDir(filepath), fileName);
                    FileOutputStream fstream = new FileOutputStream(file);
                    fstream.write(username.getBytes());
                    fstream.write(password.getBytes());
                    fstream.close();
                    Toast.makeText(getApplicationContext(), "Details Saved Successfully", Toast.LENGTH_SHORT).show();
                    intent = new Intent(MainActivity.this, Details.class);
                    startActivity(intent);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        });
    }
}

```
## activity_details.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    >

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/t1"
        android:padding="100px"/>



    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/b"
        android:text="Back"
        />

</LinearLayout>
```
## Details.java
```java
package com.example.external_storage;

import androidx.appcompat.app.AppCompatActivity;
import android.content.Intent;
import android.os.Bundle;
import android.os.Environment;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.io.InputStreamReader;

public class Details extends AppCompatActivity {
    private TextView t1;
    private Button b;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_details);

        b = findViewById(R.id.b);
        t1 = findViewById(R.id.t1);

        try {
            String fileName = "user_credentials.txt";
            String filepath = "files";
            File file = new File(getExternalFilesDir(filepath), fileName);

            if (file.exists()) {
                FileInputStream fis = new FileInputStream(file);
                InputStreamReader inputStreamReader = new InputStreamReader(fis);
                BufferedReader bufferedReader = new BufferedReader(inputStreamReader);

                StringBuilder stringBuilder = new StringBuilder();
                String line;

                while ((line = bufferedReader.readLine()) != null) {
                    stringBuilder.append(line).append("\n");
                }

                bufferedReader.close();
                t1.setText(stringBuilder.toString());

            } else {
                t1.setText("File does not exist.");
            }
        } catch (Exception e) {
            e.printStackTrace();
            t1.setText("An error occurred while reading the file.");
        }

        b.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent i = new Intent(Details.this, MainActivity.class);
                startActivity(i);
            }
        });
    }
}

```



