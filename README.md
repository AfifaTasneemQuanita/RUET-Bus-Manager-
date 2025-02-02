
# RUET BUS MANAGER 
This application was created to set the alarm to notify when the bus arrived in the desired time.

## Installation

To install my app in android devices follow the steps:

```bash
 1. Open Build Folder.
 2.Install RUET_BUS_MANAGER.apk file.
  
```
    
## Features

- The opening page of the app will have three segmant buttons Morning, Afternon and Evening 
- Each of the button on pressing will take us to a new page.
- The new page will contain the Schedule of the bus and also the buttons to set the alarms for and it will show at the screen when your bus will come to your desired destination.
- There is another button right below the set alarm button cancel alarm button to cancel the alarm if wishes to.
- Finally after setting the alarm it will notify you when your bus will come.


## Tools
Platform: Android Studio 

Programming Language: JAVA
## Lessons Learned

What did you learn while building this project? What challenges did you face and how did you overcome them?


## Code
//Code for Main JAVA function (The first page):


package com.example.ruet_bus_manager;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {
    private Button Morning;
    private Button Afternoon;
    private Button Evening;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Morning = (Button) findViewById(R.id.Morning);//activating morning button
        Morning.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                openMorningSchedule();
            }
        });
        Afternoon = (Button) findViewById(R.id.Afternoon);//activating afternoon button
        Afternoon.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                openAfternoonSchedule();
            }
        });
        Evening = (Button) findViewById(R.id.Evening);//activating evening button
        Evening.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                openEveningSchedule();
            }
        });

    }
    public void openMorningSchedule(){
        Intent intent = new Intent(this, Morning.class);//to go to a new activity named Morning
        startActivity(intent);
    }
    public void openAfternoonSchedule()
    {
        Intent intent = new Intent(this,Afternoon.class );//to go to a new activity named afternoon
        startActivity(intent);
    }
    public void openEveningSchedule()
    {
        Intent intent = new Intent(this, Evening.class);//to go to a new activity named evening
        startActivity(intent);
    }
}

//Code for the Morning Segmant :


package com.example.ruet_bus_manager;

import android.app.AlarmManager;
import android.app.PendingIntent;
import android.app.TimePickerDialog;
import android.content.Context;
import android.content.Intent;
import android.support.v4.app.DialogFragment;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.TimePicker;

import com.example.application.ruet_bus_manager.AlertReciever;
import com.example.application.ruet_bus_manager.TimePickerFragment;

import java.text.DateFormat;
import java.util.Calendar;

public class Morning extends AppCompatActivity implements TimePickerDialog.OnTimeSetListener {
    private Button setalarm;
    private Button cancelalarm;
    private TextView mTextView;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_morning);
        setalarm = (Button) findViewById(R.id.setalarm);
        cancelalarm = (Button) findViewById(R.id.cancelalarm);
        mTextView = (TextView) findViewById(R.id.showtime);
        cancelalarm.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                cancelAlarm();
            }
        });
        setalarm.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                DialogFragment timrpicker = new TimePickerFragment();//calling the timepicker fragment class
                timrpicker.show(getSupportFragmentManager(), "timrpicker");
            }
        });

    }

    @Override
    public void onTimeSet(TimePicker timePicker, int i, int i1) {//activating timepicker class
        Calendar c = Calendar.getInstance();
        c.set(Calendar.HOUR_OF_DAY,i);//this is for setting the hour for the alarm
        c.set(Calendar.MINUTE,i1);//this is for setting the minute for the alarm
        c.set(Calendar.SECOND,0);
        updateTimeText(c);//this will update the text which will be a countdown for the upcoming bus
        startAlarm(c);//this will set the alarm
    }
    private void updateTimeText(Calendar c)//the actual update time text sending class
    {
        String timeText = "Your Bus will be at: ";
        timeText+= DateFormat.getTimeInstance(DateFormat.SHORT).format(c.getTime());//this is to set the time for the text to be shown
        mTextView.setText(timeText);
    }
    private void startAlarm(Calendar c){//this class is to set the alarm
        AlarmManager alarmManager = (AlarmManager) getSystemService(Context.ALARM_SERVICE);//calling the build in alarm manager function
        Intent intent = new Intent(this, AlertReciever.class);
        PendingIntent pendingIntent = PendingIntent.getBroadcast(this, 1,intent,0);//calling the build in broadcast function
        if  (c.before(Calendar.getInstance())) {
            c.add(Calendar.DATE, 1);
        }

        alarmManager.setExact(AlarmManager.RTC_WAKEUP, c.getTimeInMillis(), pendingIntent);//this will set the format for time
    }
private void cancelAlarm(){//this class is for if we want to cancel the alarm
    AlarmManager alarmManager = (AlarmManager) getSystemService(Context.ALARM_SERVICE);
    Intent intent = new Intent(this, AlertReciever.class);
    PendingIntent pendingIntent = PendingIntent.getBroadcast(this, 1,intent,0);
    alarmManager.cancel(pendingIntent);
    mTextView.setText("Alarm Canceled");
}

}


//Code for the Afternoon Segmant:


package com.example.ruet_bus_manager;

import android.annotation.SuppressLint;
import android.app.AlarmManager;
import android.app.PendingIntent;
import android.app.TimePickerDialog;
import android.content.Context;
import android.content.Intent;
import android.support.v4.app.DialogFragment;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.TimePicker;

import com.example.application.ruet_bus_manager.AlertReciever;
import com.example.application.ruet_bus_manager.TimePickerFragment;

import java.text.DateFormat;
import java.util.Calendar;

public class Afternoon extends AppCompatActivity implements TimePickerDialog.OnTimeSetListener {
    private Button setalarm;
    private Button cancelalarm;
    private TextView mTextView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_afternoon);
        setalarm = (Button) findViewById(R.id.setalarm);
        cancelalarm = (Button) findViewById(R.id.cancelalarm);
        mTextView = (TextView) findViewById(R.id.showtime);
        cancelalarm.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                cancelAlarm();
            }
        });
        setalarm.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                DialogFragment timrpicker = new TimePickerFragment();//calling the timepicker fragment class
                timrpicker.show(getSupportFragmentManager(), "timrpicker");
            }
        });
    }

    @Override
    public void onTimeSet(TimePicker timePicker, int i, int i1) {//activating timepicker class
        Calendar c = Calendar.getInstance();
        c.set(Calendar.HOUR_OF_DAY,i);//this is for setting the hour for the alarm
        c.set(Calendar.MINUTE,i1);//this is for setting the minute for the alarm
        c.set(Calendar.SECOND,0);
        updateTimeText(c);//this will update the text which will be a countdown for the upcoming bus
        startAlarm(c);//this will set the alarm
    }
    private void updateTimeText(Calendar c)//the actual update time text sending class
    {
        String timeText = "Your Bus will be at: ";
        timeText+= DateFormat.getTimeInstance(DateFormat.SHORT).format(c.getTime());//this is to set the time for the text to be shown
        mTextView.setText(timeText);
    }
    private void startAlarm(Calendar c){//this class is to set the alarm
        AlarmManager alarmManager = (AlarmManager) getSystemService(Context.ALARM_SERVICE);//calling the build in alarm manager function
        Intent intent = new Intent(this, AlertReciever.class);
        PendingIntent pendingIntent = PendingIntent.getBroadcast(this, 1,intent,0);//calling the build in broadcast function
        alarmManager.setExact(AlarmManager.RTC_WAKEUP, c.getTimeInMillis(),pendingIntent);//this will set the format for time
    }
    private void cancelAlarm(){//this class is for if we want to cancel the alarm
        AlarmManager alarmManager = (AlarmManager) getSystemService(Context.ALARM_SERVICE);
        Intent intent = new Intent(this, AlertReciever.class);
        PendingIntent pendingIntent = PendingIntent.getBroadcast(this, 1,intent,0);
        alarmManager.cancel(pendingIntent);
        mTextView.setText("Alarm Canceled");
    }

}


//Code for the Evening Segmant:


package com.example.ruet_bus_manager;

import android.app.AlarmManager;
import android.app.PendingIntent;
import android.app.TimePickerDialog;
import android.content.Context;
import android.content.Intent;
import android.support.v4.app.DialogFragment;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.TimePicker;

import com.example.application.ruet_bus_manager.AlertReciever;
import com.example.application.ruet_bus_manager.TimePickerFragment;

import java.text.DateFormat;
import java.util.Calendar;

public class Evening extends AppCompatActivity implements TimePickerDialog.OnTimeSetListener  {
    private Button setalarm;
    private Button cancelalarm;
    private TextView mTextView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_evening);
        setalarm = (Button) findViewById(R.id.setalarm);
        cancelalarm = (Button) findViewById(R.id.cancelalarm);
        mTextView = (TextView) findViewById(R.id.showtime);
        cancelalarm.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                cancelAlarm();
            }
        });
        setalarm.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                DialogFragment timrpicker = new TimePickerFragment();//calling the timepicker fragment class
                timrpicker.show(getSupportFragmentManager(), "timrpicker");
            }

        });
    }
    @Override
    public void onTimeSet(TimePicker timePicker, int i, int i1) {//activating timepicker class
        Calendar c = Calendar.getInstance();
        c.set(Calendar.HOUR_OF_DAY,i);//this is for setting the hour for the alarm
        c.set(Calendar.MINUTE,i1);//this is for setting the minute for the alarm
        c.set(Calendar.SECOND,0);
        updateTimeText(c);//this will update the text which will be a countdown for the upcoming bus
        startAlarm(c);//this will set the alarm
    }
    private void updateTimeText(Calendar c)//the actual update time text sending class
    {
        String timeText = "Your Bus will be at: ";
        timeText+= DateFormat.getTimeInstance(DateFormat.SHORT).format(c.getTime());//this is to set the time for the text to be shown
        mTextView.setText(timeText);
    }
    private void startAlarm(Calendar c){//this class is to set the alarm
        AlarmManager alarmManager = (AlarmManager) getSystemService(Context.ALARM_SERVICE);//calling the build in alarm manager function
        Intent intent = new Intent(this, AlertReciever.class);
        PendingIntent pendingIntent = PendingIntent.getBroadcast(this, 1,intent,0);//calling the build in broadcast function
        if  (c.before(Calendar.getInstance())) {
            c.add(Calendar.DATE, 1);
        }

        alarmManager.setExact(AlarmManager.RTC_WAKEUP, c.getTimeInMillis(), pendingIntent);//this will set the format for time
    }
    private void cancelAlarm(){//this class is for if we want to cancel the alarm
        AlarmManager alarmManager = (AlarmManager) getSystemService(Context.ALARM_SERVICE);
        Intent intent = new Intent(this, AlertReciever.class);
        PendingIntent pendingIntent = PendingIntent.getBroadcast(this, 1,intent,0);
        alarmManager.cancel(pendingIntent);
        mTextView.setText("Alarm Canceled");
    }
}


//Code for TimePickerFragment class:

package com.example.application.ruet_bus_manager;
import android.app.Dialog;
import android.app.TimePickerDialog;
import android.os.Bundle;
import android.support.annotation.NonNull;
import android.support.v4.app.DialogFragment;
import android.text.format.DateFormat;

import java.util.Calendar;

public class TimePickerFragment extends DialogFragment {//this class to set the scheduled time for the bus it will be done manually by seeing the actual schedule
    @NonNull
    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState){
        Calendar c = Calendar.getInstance();
        int hour = c.get(Calendar.HOUR_OF_DAY);//to set the time as required
        int minute = c.get(Calendar.MINUTE);
        TimePickerDialog timePickerDialog = new TimePickerDialog(getActivity(), (TimePickerDialog.OnTimeSetListener) getActivity(),hour,minute, DateFormat.is24HourFormat(getActivity()));//this function will get the time from the device
        return timePickerDialog;
    }

}

//Code for NotificationHelper class:

package com.example.application.ruet_bus_manager;
import android.annotation.TargetApi;
import android.app.Notification;
import android.app.NotificationChannel;
import android.app.NotificationManager;
import android.content.Context;
import android.content.ContextWrapper;
import android.os.Build;
import android.support.v4.app.NotificationCompat;

import com.example.ruet_bus_manager.R;

public class NotificationHelper extends ContextWrapper {//this class is to set the notification that is to be sent
    public static final String ID = "cID";
    public static final String Name = "Channel";
    private NotificationManager mManager;

    public NotificationHelper(Context base) {
        super(base);
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            createChannel();
        }

    }
    @TargetApi(Build.VERSION_CODES.O)
    private void createChannel() {//this class is to customizing the notification channel

            NotificationChannel channel = new NotificationChannel(ID, Name, NotificationManager.IMPORTANCE_DEFAULT);
            channel.enableVibration(true);
            channel.enableLights(true);
            channel.setLightColor(R.color.white);
            channel.setLockscreenVisibility(Notification.VISIBILITY_PRIVATE);

            getManager().createNotificationChannel(channel);

    }

    public NotificationManager getManager() {//this class will activate the build in notification service
        if (mManager == null) {
            mManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
        }
        return mManager;
    }


    public NotificationCompat.Builder getChannelNotification() { //this class is for customizing the notification
        return new NotificationCompat.Builder(getApplicationContext(), ID)
                .setContentTitle("Check New Notifications:")
                .setContentText("Your Bus is here!")//the notification text
                .setPriority(NotificationCompat.PRIORITY_HIGH)// setting the priority of the notification
                .setOnlyAlertOnce(true)//how many time we want to send the notification
                .setSmallIcon(android.R.drawable.presence_away);
    }


}

//Code for AlertReciever Class:


package com.example.application.ruet_bus_manager;
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.support.v4.app.NotificationCompat;

public class AlertReciever extends BroadcastReceiver {//this class combined the notification helper class to alert the user
    @Override
    public void onReceive(Context context, Intent intent) {
        NotificationHelper notificationHelper = new NotificationHelper(context);
        NotificationCompat.Builder nb = notificationHelper.getChannelNotification();//building the notification class
        notificationHelper.getManager().notify(1,nb.build());//calling the notification helper class
    }
}


//code for the XML (The first page):


<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="30sp"
        android:text="                      Welcome To RUET BUS MANAGER "
        android:textSize="15sp"/>

    <Button
        android:id="@+id/Morning"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true"
        android:text="@string/morning"
        android:textSize="20sp" />

    <Button
        android:id="@+id/Afternoon"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/Morning"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:text="Afternoon+"
        android:textSize="20sp"/>
    <Button
        android:id="@+id/Evening"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/Afternoon"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:text="Evening+"
        android:textSize="20sp"/>


</RelativeLayout>


//Code for XML (Morning Segmant):


<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".Morning">
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="30sp"
        android:text="                                  Morning Schedule "
        android:textSize="16dp"/>
    <TextView
        android:id="@+id/showtime"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Your Bus will be at: "
        android:layout_above="@+id/setalarm"
        android:textSize="20sp"/>
    <Button
        android:id="@+id/setalarm"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Set Alarm"
        android:textSize="20sp"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true"/>

    <Button
        android:id="@+id/cancelalarm"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/setalarm"
        android:layout_centerHorizontal="true"
        android:text="Cancel Alarm"
        android:textSize="20sp" />

    <TextView
        android:id="@+id/showschedule"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_above="@+id/showtime"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="57dp"
        android:text="Alupotti at 7.04AM -> Bazar at 7.10AM -> Sonadighir More at 7.15AM -> CNB at 7.25AM -> Court Station at 7.30AM -> Laxmipur at 7.35AM -> Bohorompur at 7.40AM -> Bornali More at 7.45AM -> Railgate at 7.50AM -> RUET at 7.55AM"
        android:textSize="20sp" />


</RelativeLayout>


//Code for XML(Afternoon Segmant):


<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".Morning">
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="30sp"
        android:text="                                 Afternoon Schedule "
        android:textSize="16dp"/>
    <TextView
        android:id="@+id/showtime"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Your Bus will be at: "
        android:layout_above="@+id/setalarm"
        android:textSize="20sp"/>
    <Button
        android:id="@+id/setalarm"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Set Alarm"
        android:textSize="20sp"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true"/>

    <Button
        android:id="@+id/cancelalarm"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/setalarm"
        android:layout_centerHorizontal="true"
        android:text="Cancel Alarm"
        android:textSize="20sp" />


</RelativeLayout>

//Code for XML(Evening Segmant):


<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".Morning">
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="30sp"
        android:text="                                  Evening Schedule "
        android:textSize="16dp"/>
    <TextView
        android:id="@+id/showtime"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Your Bus will be at: "
        android:layout_above="@+id/setalarm"
        android:textSize="20sp"/>
    <Button
        android:id="@+id/setalarm"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Set Alarm"
        android:textSize="20sp"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true"/>

    <Button
        android:id="@+id/cancelalarm"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/setalarm"
        android:layout_centerHorizontal="true"
        android:text="Cancel Alarm"
        android:textSize="20sp" />


</RelativeLayout>