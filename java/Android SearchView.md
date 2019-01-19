# Java: Android SearchView


```java
    <item android:id="@+id/menu_item_search"
        android:title="@android:string/search_go"
        android:icon="@android:drawable/ic_menu_search"
        android:showAsAction="ifRoom"
        android:actionViewClass="android.widget.SearchView"
        android:imeOptions="actionSearch"
        android:orderInCategory="1" />
```


```java
@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.cities_menu, menu);
        MenuItem help = menu.findItem(R.id.menu_item_help);
        if (help != null) {
            Utils.prepareHelpMenuItem(this, help);
        }
        MenuItem searchMenu = menu.findItem(R.id.menu_item_search);
        mSearchView = (SearchView) searchMenu.getActionView();
        mSearchView.setImeOptions(EditorInfo.IME_FLAG_NO_EXTRACT_UI);
        mSearchView.setOnSearchClickListener(new OnClickListener() {
            @Override
            public void onClick(View arg0) {
                mSearchMode = true;
            }
        });
        mSearchView.setOnCloseListener(new SearchView.OnCloseListener() {
            @Override
            public boolean onClose() {
                mSearchMode = false;
                return false;
            }
        });
        if (mSearchView != null) {
            mSearchView.setOnQueryTextListener(this);
            mSearchView.setQuery(mQueryTextBuffer.toString(), false);
            if (mSearchMode) {
                mSearchView.requestFocus();
                mSearchView.setIconified(false);
            }
        }
        return super.onCreateOptionsMenu(menu);
    }
```


