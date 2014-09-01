title: 使用ContentProvider管理App数据
date: 2014-07-07 13:36:43
categories: Android
tags: [Android]
---
##工具类
- Column
用于管理字段
```java
public class Column {
	// 约束
	public static enum Constraint {
		UNIQUE("UNIQUE"), NOT("NOT"), NULL("NULL"), CHECK("CHECK"), FOREIGN_KEY(
				"FOREIGN KEY"), PRIMARY_KYE("PRIMARY KEY");
		private String value;

		private Constraint(String value) {
			this.value = value;
		}

		@Override
		public String toString() {
			return value;
		}
	}

	// 数据类型
	public static enum DataType {
		NULL, INTEGER, REAL, TEXT, BLOB
	}

	private String mColumnName;
	private Constraint mConstraint;
	private DataType mDataType;

	public Column(String columnName, Constraint constraint, DataType dataType) {
		mColumnName = columnName;
		mConstraint = constraint;
		mDataType = dataType;
	}

	public String getColumnName() {
		return mColumnName;
	}

	public Constraint getConstraint() {
		return mConstraint;
	}

	public DataType getDataType() {
		return mDataType;
	}
}
```
- SQLiteTable
用于表管理
```java
public class SQLiteTable {
	String mTableName;
	ArrayList<Column> mColumnsDefinitions = new ArrayList<Column>();

	public String getTableName() {
		return mTableName;
	}

	// 自动添加主键_ID
	public SQLiteTable(String tableName) {
		mTableName = tableName;
		mColumnsDefinitions.add(new Column(BaseColumns._ID,
				Column.Constraint.PRIMARY_KYE, Column.DataType.INTEGER));
	}

	public SQLiteTable addColumn(Column columnsDefinition) {
		mColumnsDefinitions.add(columnsDefinition);
		return this;
	}

	public SQLiteTable addColumn(String columnName, Column.DataType dataType) {
		mColumnsDefinitions.add(new Column(columnName, null, dataType));
		return this;
	}

	public SQLiteTable addColumn(String columnName,
			Column.Constraint constraint, Column.DataType dataType) {
		mColumnsDefinitions.add(new Column(columnName, constraint, dataType));
		return this;
	}

	// 在SQLite中创建本表
	public void create(SQLiteDatabase db) {
		String formatter = " %s";
		StringBuilder stringBuilder = new StringBuilder();
		stringBuilder.append("CREATE TABLE IF NOT EXISTS ");
		stringBuilder.append(mTableName);
		stringBuilder.append("(");
		int columnCount = mColumnsDefinitions.size();
		int index = 0;
		for (Column columnsDefinition : mColumnsDefinitions) {
			stringBuilder.append(columnsDefinition.getColumnName()).append(
					String.format(formatter, columnsDefinition.getDataType()
							.name()));
			Column.Constraint constraint = columnsDefinition.getConstraint();

			if (constraint != null) {
				stringBuilder.append(String.format(formatter,
						constraint.toString()));
			}
			if (index < columnCount - 1) {
				stringBuilder.append(",");
			}
			index++;
		}
		stringBuilder.append(");");
		db.execSQL(stringBuilder.toString());
	}

	// 在SQLite中删除本表
	public void delete(final SQLiteDatabase db) {
		db.execSQL("DROP TABLE IF EXISTS " + mTableName);
	}
}
```

##数据库操作类
```java
public abstract class BaseDataHelper {
	private Context mContext;

	public BaseDataHelper(Context context) {
		mContext = context;
	}

	public Context getContext() {
		return mContext;
	}

	protected abstract Uri getContentUri();

	public void notifyChange() {
		mContext.getContentResolver().notifyChange(getContentUri(), null);
	}

	protected final Cursor query(Uri uri, String[] projection,
			String selection, String[] selectionArgs, String sortOrder) {
		return mContext.getContentResolver().query(uri, projection, selection,
				selectionArgs, sortOrder);
	}

	protected final Cursor query(String[] projection, String selection,
			String[] selectionArgs, String sortOrder) {
		return mContext.getContentResolver().query(getContentUri(), projection,
				selection, selectionArgs, sortOrder);
	}

	protected final Uri insert(ContentValues values) {
		return mContext.getContentResolver().insert(getContentUri(), values);
	}

	protected final int bulkInsert(ContentValues[] values) {
		return mContext.getContentResolver()
				.bulkInsert(getContentUri(), values);
	}

	protected final int update(ContentValues values, String where,
			String[] whereArgs) {
		return mContext.getContentResolver().update(getContentUri(), values,
				where, whereArgs);
	}

	protected final int delete(Uri uri, String selection, String[] selectionArgs) {
		return mContext.getContentResolver().delete(getContentUri(), selection,
				selectionArgs);
	}

	protected final Cursor getList(String[] projection, String selection,
			String[] selectionArgs, String sortOrder) {
		return mContext.getContentResolver().query(getContentUri(), projection,
				selection, selectionArgs, sortOrder);
	}

	public CursorLoader getCursorLoader(Context context) {
		return getCursorLoader(context, null, null, null, null);
	}

	protected final CursorLoader getCursorLoader(Context context,
			String[] projection, String selection, String[] selectionArgs,
			String sortOrder) {
		return new CursorLoader(context, getContentUri(), projection,
				selection, selectionArgs, sortOrder);
	}
}
```
