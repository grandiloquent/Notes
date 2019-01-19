# Java: Android SQLite


```java
package euphoria.psycho.download;


import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;
import android.os.Environment;

import java.io.File;
import java.util.ArrayList;
import java.util.List;

import euphoria.psycho.download.persistence.DownloadInfo;

public class Database extends SQLiteOpenHelper {
    private static final String COLUMN_CREATED_AT = "createdAt";
    private static final String COLUMN_CURRENT_BYTES = "currentBytes";
    private static final String COLUMN_FILENAME = "filename";
    private static final String COLUMN_FINISHED = "finished";
    private static final String COLUMN_ID = "_id";
    private static final String COLUMN_MESSAGE = "message";
    private static final String COLUMN_STATUS = "status";
    private static final String COLUMN_TOTAL_BYTES = "totalBytes";
    private static final String COLUMN_URL = "url";
    private static final String DATABASE_NAME = "download.db";
    private static final int DATABASE_VERSION = 1;
    private static final String TABLE_NAME = "list";
    private static Database sDatabase;

    public Database(Context context) {
        super(context, new File(Environment.getExternalStorageDirectory(), DATABASE_NAME).getAbsolutePath(), null, DATABASE_VERSION);

    }

    public void delete(DownloadInfo info) {
        getWritableDatabase().delete(TABLE_NAME, "_id=?", new String[]{Long.toString(info.id)});
    }

    public DownloadInfo fetch(long id) {
        Cursor cursor = getReadableDatabase().rawQuery("select * from List where _id=?", new String[]{Long.toString(id)});
        try {

            if (cursor.moveToNext()) {
                DownloadInfo downloadInfo = new DownloadInfo();
                downloadInfo.id = cursor.getLong(0);
                downloadInfo.fileName = cursor.getString(1);
                downloadInfo.url = cursor.getString(2);
                downloadInfo.currentBytes = cursor.getLong(3);
                downloadInfo.totalBytes = cursor.getLong(4);
                return downloadInfo;
            }

        } finally {
            cursor.close();
        }
        return null;
    }

    public List<DownloadInfo> fetchAll() {
        return fetchAllInternal("select * from list");
    }

    private List<DownloadInfo> fetchAllInternal(String cmd) {
        Cursor cursor = getReadableDatabase().rawQuery(cmd, null);
        List<DownloadInfo> list = new ArrayList<>();
        try {

            while (cursor.moveToNext()) {
                DownloadInfo downloadInfo = new DownloadInfo();
                downloadInfo.id = cursor.getLong(0);
                downloadInfo.fileName = cursor.getString(1);
                downloadInfo.url = cursor.getString(2);
                downloadInfo.currentBytes = cursor.getLong(3);
                downloadInfo.totalBytes = cursor.getLong(4);
                list.add(downloadInfo);
            }

        } finally {
            cursor.close();
        }
        return list;
    }

    public List<DownloadInfo> fetchAllTask() {
        return fetchAllInternal("select * from list where finished=0 or finished is null");
    }

    public long insert(DownloadInfo downloadInfo) {
        ContentValues values = new ContentValues();
        values.put("fileName", downloadInfo.fileName);
        values.put("url", downloadInfo.url);

        return getWritableDatabase().insertWithOnConflict("List", null, values, SQLiteDatabase.CONFLICT_IGNORE);
    }

    public synchronized void update(DownloadInfo downloadInfo) {
        ContentValues values = new ContentValues();

        values.put(COLUMN_CURRENT_BYTES, downloadInfo.currentBytes);
        values.put(COLUMN_TOTAL_BYTES, downloadInfo.totalBytes);
        values.put(COLUMN_FINISHED, downloadInfo.finished ? 1 : 0);
        values.put(COLUMN_STATUS, downloadInfo.status);
        values.put(COLUMN_MESSAGE, downloadInfo.message);
        getWritableDatabase().updateWithOnConflict(TABLE_NAME,
                values,
                COLUMN_ID + "=?",
                new String[]{Long.toString(downloadInfo.id)}, SQLiteDatabase.CONFLICT_IGNORE);
    }

    public static synchronized Database getInstance(Context context) {
        if (sDatabase == null) {
            sDatabase = new Database(context);
        }
        return sDatabase;
    }

    @Override
    public void onCreate(SQLiteDatabase db) {

        String sb = "CREATE TABLE IF NOT EXISTS List" + '\n' +
                "(      [" + COLUMN_ID + "] INTEGER PRIMARY KEY AUTOINCREMENT," + '\n' +
                "       [" + COLUMN_FILENAME + "] TEXT NOT NULL UNIQUE," + '\n' +
                "       [" + COLUMN_URL + "] TEXT NOT NULL UNIQUE," + '\n' +
                "       [" + COLUMN_CURRENT_BYTES + "] INTEGENR," + '\n' +
                "       [" + COLUMN_TOTAL_BYTES + "] INTEGENR," + '\n' +
                "       [" + COLUMN_CREATED_AT + "] DATETIME DEFAULT CURRENT_TIMESTAMP," + '\n' +
                "       [" + COLUMN_FINISHED + "] INTEGENR," + '\n' +
                "       [" + COLUMN_STATUS + "] INTEGENR," + '\n' +
                "       [" + COLUMN_MESSAGE + "] TEXT" + '\n' +

                ");" + '\n';
        db.execSQL(sb);

    }


    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {

    }
}

```



