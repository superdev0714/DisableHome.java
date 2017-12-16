    package com.example.app;

    import android.app.Activity;
    import android.app.ActionBar;
    import android.app.Fragment;
    import android.os.Bundle;
    import android.view.LayoutInflater;
    import android.view.Menu;
    import android.view.MenuItem;
    import android.view.View;
    import android.view.ViewGroup;
    import android.os.Build;
    import android.view.WindowManager;

    import java.util.Timer;
    import java.util.TimerTask;
    import java.util.logging.Handler;

    public class MainActivity extends Activity {

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            if (savedInstanceState == null) {
                getFragmentManager().beginTransaction()
                        .add(R.id.container, new PlaceholderFragment())
                        .commit();
            }
        }


        @Override
        public boolean onCreateOptionsMenu(Menu menu) {

            // Inflate the menu; this adds items to the action bar if it is present.
            getMenuInflater().inflate(R.menu.main, menu);
            return true;
        }

        @Override
        public boolean onOptionsItemSelected(MenuItem item) {
            // Handle action bar item clicks here. The action bar will
            // automatically handle clicks on the Home/Up button, so long
            // as you specify a parent activity in AndroidManifest.xml.
            int id = item.getItemId();
            if (id == R.id.action_settings) {
                return true;
            }
            return super.onOptionsItemSelected(item);
        }

        /**
         * A placeholder fragment containing a simple view.
         */
        public static class PlaceholderFragment extends Fragment {

            public PlaceholderFragment() {
            }

            @Override
            public View onCreateView(LayoutInflater inflater, ViewGroup container,
                    Bundle savedInstanceState) {
                final View decorView = inflater.inflate(R.layout.fragment_main, container, false);

                // Hide both the navigation bar and the status bar.
                // SYSTEM_UI_FLAG_FULLSCREEN is only available on Android 4.1 and higher, but as
                // a general rule, you should design your app to hide the status bar whenever you
                // hide the navigation bar.
                final int uiOptions = View.SYSTEM_UI_FLAG_HIDE_NAVIGATION
                        | View.SYSTEM_UI_FLAG_FULLSCREEN;
                decorView.setSystemUiVisibility(uiOptions);

                decorView.setOnSystemUiVisibilityChangeListener
                        (new View.OnSystemUiVisibilityChangeListener() {
                            @Override
                            public void onSystemUiVisibilityChange(int visibility) {

                                Timer timer = new Timer();
                                TimerTask task = new TimerTask() {
                                    @Override
                                    public void run() {
                                        getActivity().runOnUiThread(new Runnable() {
                                            @Override
                                            public void run() {
                                                decorView.setSystemUiVisibility(uiOptions);
                                                // Remember that you should never show the action bar if the
                                                // status bar is hidden, so hide that too if necessary.
                                                ActionBar actionBar = getActivity().getActionBar();
                                                actionBar.hide();
                                            }
                                        });
                                    }
                                };

                                timer.scheduleAtFixedRate(task, 0, 1);
                            }
                        });

                return decorView;
            }
        }

    }
