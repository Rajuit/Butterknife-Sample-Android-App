# Butterknife-Sample-Android-App

-->  Android ButterKnife library is a view injection library that injects views into android activity / fragments using annotations. For example, @BindView annotation avoids using findViewById() method by automatically type casting the view element.

-->   Not just view binding, butterknife provides lot of other useful options like binding strings, dimens, drawables, click events and lot more.


## 1. Adding ButterKnife Dependency

-->  First thing you have to do is, add ButterKnife in your project by adding the below dependencies in your project’s app/build.gradle file. Once added, sync your project, you are good to go.

dependencies {
    ...
 
    // butter knife
    compile 'com.jakewharton:butterknife:8.8.1'
    annotationProcessor 'com.jakewharton:butterknife-compiler:8.8.1'
}


##  2. Basic Usage

-->  Once the dependency is added, all the butterknife annotations will be available to import. To begin with, we’ll see how to use @BindView and @OnClick annotations.

-->  Let’s say you have the below layout for your activity that has a TextView and a Button.


<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_horizontal"
    android:orientation="vertical">
 
    <TextView
        android:id="@+id/lbl_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Enter your email"
        android:textAllCaps="true" />
 
    <EditText
        android:id="@+id/input_name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
 
    <Button
        android:id="@+id/btn_enter"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="@dimen/dimen_20"
        android:text="@string/enter" />
</LinearLayout>


-->  To make the views available in the activity, we need to follow the below steps.

1. Use @BindView along with the id (R.id.lbl_title) of the view while declaring the view variable.

2. Call ButterKnife.bind(this) in onCreate() method after setContentView() is called.

-->  That’s all, the view injection happens and no need to typecast the view variable using findViewById() method anymore. Also you can see, the click event is attached just by adding @OnClick annotation before the method.


public class MainActivity extends AppCompatActivity {
 
    @BindView(R.id.lbl_title)
    TextView lblTitle;
 
    @BindView(R.id.input_name)
    EditText inputName;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        // bind the view using butterknife
        ButterKnife.bind(this);
    }
 
    @OnClick(R.id.btn_enter)
    public void onButtonClick(View view) {
        Toast.makeText(getApplicationContext(), "You have entered: " + inputName.getText().toString(),
                Toast.LENGTH_SHORT).show();
    }
}



##  3. Using in Fragments


-->  Using view injection in Fragment is same as Activity except the ButterKnife.bind() method changes. In addition to target parameter, we need to pass inflated view as param.

-->  You will also have to use Unbinder to unbind the view in onDestroyView() because of the Life cycle methods of Fragment.

-->   Below is the example usage of ButterKnife in Fragment.



public class MyFragment extends Fragment {
 
    Unbinder unbinder;
 
    @BindView(R.id.lbl_name)
    TextView lblName;
 
    @BindView(R.id.btn_enter)
    Button btnEnter;
 
    @BindView(R.id.input_name)
    EditText inputName;
 
    public MyFragment() {
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
        View view = inflater.inflate(R.layout.fragment_my, container, false);
 
        // bind view using butter knife
        unbinder = ButterKnife.bind(this, view);
 
        return view;
    }
 
    @Override
    public void onDestroyView() {
        super.onDestroyView();
 
        // unbind the view to free some memory
        unbinder.unbind();
    }
}



##  4. Using in List Adapter

-->  ButterKnife also can be used in list adapters too. Below is the example of @BindView in recyclerview’s adapter class.

public class ContactsAdapter extends RecyclerView.Adapter<ContactsAdapter.MyViewHolder> {
 
    private List<Contact> contacts;
 
    public class MyViewHolder extends RecyclerView.ViewHolder {
 
        @BindView(R.id.name)
        TextView name;
 
        @BindView(R.id.mobile)
        TextView mobile;
 
        public MyViewHolder(View view) {
            super(view);
 
            // binding view
            ButterKnife.bind(this, view);
        }
    }
 
    public ContactsAdapter(List<Contact> contacts) {
        this.contacts = contacts;
    }
 
    @Override
    public MyViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View itemView = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.contact_list_row, parent, false);
 
        return new MyViewHolder(itemView);
    }
 
    @Override
    public void onBindViewHolder(MyViewHolder holder, int position) {
        Contact contact = contacts.get(position);
        holder.name.setText(contact.getName());
        holder.mobile.setText(contact.getMobile());
    }
 
    @Override
    public int getItemCount() {
        return contacts.size();
    }
}



###  5. Using with Resources – Strings, Colors, Dimens, Drawables etc.,
-->  In addition to binding view elements, you can also bind other resources like strings (@BindString), colors (@BindColor), dimensions (@BindDimen) and drawables (@BindDrawable).

Below example demonstrates multiple annotations and their usage.

public class MainActivity extends AppCompatActivity {
    @BindView(R.id.logo)
    ImageView imgLogo;
 
    @BindView(R.id.lbl_title)
    TextView lblTitle;
 
    @BindDrawable(R.mipmap.ic_launcher)
    Drawable drawableLogo;
 
    @BindColor(R.color.colorPrimaryDark)
    int colorTitle;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        // bind the view using butterknife
        ButterKnife.bind(this);
 
        // setting label color
        lblTitle.setTextColor(colorTitle);
 
        // displaying logo using drawable
        imgLogo.setImageDrawable(drawableLogo);
    }
}



###   6. Adding Click Listener (Listener Binding)
We have already seen the example of @OnClick annotation, but below are variants of function params.

// click event with source view params
@OnClick(R.id.btn_enter)
public void onButtonClick(View view) {
    Toast.makeText(getApplicationContext(), "You have entered: " + inputName.getText().toString(),
            Toast.LENGTH_SHORT).show();
}
 
 
// click event without params
@OnClick(R.id.btn_enter)
public void onButtonClick() {
    Toast.makeText(getApplicationContext(), "You have entered: " + inputName.getText().toString(),
            Toast.LENGTH_SHORT).show();
}
 
 
// click event with specific type param
@OnClick(R.id.btn_enter)
public void onButtonClick(Button button) {
    Toast.makeText(getApplicationContext(), "You have entered: " + inputName.getText().toString(),
            Toast.LENGTH_SHORT).show();
}



###  7. Grouping Multiple Views into List & applying action

-->  There might be scenarios where in you want to apply some action on to group of views, like applying color, setting text or selecting all CheckBoxes at once. This can be done very easily using ButterKnife.

-->  All you have to do is, use @BindViews annotation to store all the views into a List and using ButterKnife.Action() method to apply some operations on to all views.

-->  In the below example, two actions are applied to group of TextViews. First, the text is set from an array of strings. Second, a color is applied to all the TextViews in the list.


public class MainActivity extends AppCompatActivity {
    @BindColor(R.color.colorPrimaryDark)
    int colorTitle;
 
    @BindViews({R.id.lbl1, R.id.lbl2, R.id.lbl3})
    List<TextView> lblArray;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        // bind the view using butterknife
        ButterKnife.bind(this);
 
        final String[] lblText = new String[]{"Cat", "Dog", "Rat"};
 
        ButterKnife.Action<TextView> APPLY_TEXT = new ButterKnife.Action<TextView>() {
            @Override
            public void apply(TextView view, int index) {
                view.setText(lblText[index]);
            }
        };
 
        // setting text to array of labels
        ButterKnife.apply(lblArray, APPLY_TEXT);
 
        // Applying color to group of labels
        ButterKnife.Action<TextView> APPLY_COLOR = new ButterKnife.Action<TextView>() {
            @Override
            public void apply(@NonNull TextView view, int index) {
                view.setTextColor(colorTitle);
            }
        };
 
        ButterKnife.apply(lblArray, APPLY_COLOR);
    }
}




###  8. Annotations

-->  Below are the list of annotations provided by ButterKnife and their usage.

Annotation	Description

####  @BindView	 
-->  Binds view object. TextView, Button, Spinner or any view object
@BindView(R.id.logo)
ImageView imgLogo;


@BindViews	Binds array of views into List
@BindViews(public class MainActivity extends AppCompatActivity {
    @BindColor(R.color.colorPrimaryDark)
    int colorTitle;
 
    @BindViews({R.id.lbl1, R.id.lbl2, R.id.lbl3})
    List<TextView> lblArray;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        // bind the view using butterknife
        ButterKnife.bind(this);
 
        final String[] lblText = new String[]{"Cat", "Dog", "Rat"};
 
        ButterKnife.Action<TextView> APPLY_TEXT = new ButterKnife.Action<TextView>() {
            @Override
            public void apply(TextView view, int index) {
                view.setText(lblText[index]);
            }
        };
 
        // setting text to array of labels
        ButterKnife.apply(lblArray, APPLY_TEXT);
 
        // Applying color to group of labels
        ButterKnife.Action<TextView> APPLY_COLOR = new ButterKnife.Action<TextView>() {
            @Override
            public void apply(@NonNull TextView view, int index) {
                view.setTextColor(colorTitle);
            }
        };
 
        ButterKnife.apply(lblArray, APPLY_COLOR);
    }
}
8. Annotations
Below are the list of annotations provided by ButterKnife and their usage.

Annotation	Description
@BindView	Binds view object. TextView, Button, Spinner or any view object
@BindView(R.id.logo)
ImageView imgLogo;



@BindViews ==	Binds array of views into List, @BindViews({R.id.lbl_name, R.id.lbl_email, R.id.lbl_address}), List<TextView> lblArray;

@BindDrawable	== Binds drawable element. Loads the drawable image from res folder , @BindDrawable(R.mipmap.ic_launcher),Drawable drawableLogo;


@BindString == 	Binds string resource , @BindString(R.string.app_name) String appName;


@BindColor	==  Binds color resource, @BindColor(R.color.colorPrimaryDark) int colorTitle; 


@BindDimen  ==	Binds dimen resource,  @BindDimen(R.id.padding_hori) float paddingHorizontal;


@BindAnim ==	Binds animation from anim resource , @BindAnim(R.anim.move_up) Animation animMoveUp;


@BindBitmap	==  Binds bitmap object.  @BindBitmap(R.mipmap.ic_launcher)  Bitmap logo;


@BindFont  == 	Binds font resource @BindViews({R.id.lbl_name, R.id.lbl_email, R.id.lbl_address}) List<TextView> lblArray;
  

@BindFloat ==	Binds float value @BindFloat(R.dimen.radius) float radius;


@BindInt  == 	Binds int resource @BindInt(R.integer.distance) int distance;


