## Solution:

### (0) creating a fragment, other files
 create fragment.xml, fragment.java extending fragment and inflate layout in 
 onCreateView().

    public class Frag_b extends Fragment {

        public Frag_b() {
            // Required empty public constructor
        }

        @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container,
                                 Bundle savedInstanceState) {
            // Inflate the layout for this fragment
            return inflater.inflate(R.layout.fragment_frag_b, container, false);
        }

    }

 
 in my app , i used linerlayout with vertical orientation and an id as root view.


## (1) Dynamic Fragments:
As we know when an app launches, the androidOS first calls the activity to show
up(the one having ` "android.intent.action.MAIN" ` permission ). so to display 
a fragment in runtime we use a class called FragmentManager(Read more about its 
features in [here](https://developer.android.com/reference/android/app/FragmentManager.html).So, to attach a fragment:

1 - create  **global objects of both the fragments and fragment manager**  in main activity.  
2 - create **global fragment tags** : they are very important for 
    restorability of same fragment objects. the will be used as tags to access/ identify strings.  
3 - addition via fragment manager: So here comes the first use-case of fragment manager : to add fragments dynamically(at run time). here is the code for it:  
    

    public class MainActivity extends AppCompatActivity implements FragAHandler,FragBHandler {
        LinearLayout layRoot;

        FragmentManager manager=getSupportFragmentManager();

        public static  final String FRAG_A_TAG="FRAG_A";
        public static  final String FRAG_B_TAG="FRAG_B";


        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            layRoot=findViewById(R.id.layout_root);
            manager.beginTransaction().add(R.id.layout_root,new Frag_a(),FRAG_A_TAG).commit();
            manager.beginTransaction().add(R.id.layout_root,new Frag_b(),FRAG_B_TAG).commit();

        }

    }


    

# (2) Fragment Communication:
 
 As indicated [here](https://stackoverflow.com/a/12683615/7500651), fragment communication is a 
 4 layered process:  
 

 - an interface defines the functions(the task that fragment wants to perform).
 - the activity implements them.
 - the fragment gets an instance of  this interface(in the form of activity's context).
 - fragment uses this instance according to its need .  

{in case of fragment-fragment communication , 2 more small steps are added to it:}  

 - the fragment sends a signal to activity
 - the activity then handles the signal and sends it to other fragment  
  
thus if a fragment `FragA` wants the main activity to show notification on a button click(present inside fragment ):  

 - create an interface  `FragAcall` having function `void showNotif()`.
 - Implement it in `MainActivity`.(and define whatever you wish that your activity should perform on receiving click from fragment A .i.e, showing a notification)
 - in `fragA` create an object of interface `FragAcall` (`FragAcall callObj`).
 - in `fragA`, override a method named **`onAttach(context)`**.cast this context to FragAcall' Object.
 - inside  your button's on click listener, call `callObj.showNotiff()`.

similarly for a 2 way fragment-fragment communication, I implemented the code using these steps:
 
 - created interfaces `FragAHandler` and `FragBHandler`.

    public interface FragAHandler {
            void addFrag1();
            void closeFrag1();
        }
    public interface FragBHandler {
             void addFrag2();
             void closeFrag2();
        }


 - implemented these in `MainActivity` :

        public class MainActivity extends AppCompatActivity implementsFragAHandler,FragBHandler {
    
        LinearLayout layRoot;
        FragmentManager manager=getSupportFragmentManager();
        public static  final String FRAG_A_TAG="FRAG_A";
        public static  final String FRAG_B_TAG="FRAG_B";
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            layRoot=findViewById(R.id.layout_root);
            manager.beginTransaction().add(R.id.layout_root,new Frag_a(),FRAG_A_TAG).commit();
        }
    
        @Override
        public void addFrag1() {
            if(manager.findFragmentByTag(FRAG_A_TAG)==null){
                manager.beginTransaction().add(R.id.layout_root,new Frag_a(),FRAG_A_TAG).commit();
            }
            else{
                Toast.makeText(MainActivity.this,"frag1 Already present",Toast.LENGTH_SHORT);
            }
        }
    
        @Override
        public void closeFrag1() {
            Frag_a fragA= (Frag_a) manager.findFragmentByTag(FRAG_A_TAG);
            if(fragA!=null){
                manager.beginTransaction().remove(fragA).commit();
            }
            else{
                Toast.makeText(MainActivity.this,"frag1 Already not there",Toast.LENGTH_SHORT);
            }
    
        }
    
        @Override
        public void addFrag2() {
    
            if(manager.findFragmentByTag(FRAG_B_TAG)==null){
                manager.beginTransaction().add(R.id.layout_root,new Frag_b(),FRAG_B_TAG).commit();
            }
            else{
                Toast.makeText(MainActivity.this,"frag2 Already present",Toast.LENGTH_SHORT);
            }
        }
    
        @Override
        public void closeFrag2() {
            Frag_b frag_b= (Frag_b) manager.findFragmentByTag(FRAG_B_TAG);
            
            if(frag_b!=null){
                manager.beginTransaction().remove(frag_b).commit();
            }
            else{
                Toast.makeText(MainActivity.this,"frag2 Already not there",Toast.LENGTH_SHORT);
    
            }
    
        }}

in this code `manager.findFragByTag()` is being used to  check weather a fragment
is already being displayed or not,since we only want to open one single instance
of the other fragment. for its detailed use, see [this](https://developer.android.com/reference/android/app/Fragment.html#BackStack)


- inside fragments, attach the mainActivity's class context to your handler obj and use it inside buttons.



     public class Frag_a extends Fragment {
            
            FragBHandler fragBHandler;
    
            public Frag_a() {
                // Required empty public constructor
            }
    
            @Override
            public void onAttach(Context context) {
                //context is the activity's context.
                super.onAttach(context);
    
                try {
                    // This makes sure that the container activity has implemented
                    // the callback interface. If not, it throws an exception
                    fragBHandler = (FragBHandler) context;
                } catch (ClassCastException e) {
                    e.printStackTrace();
                    Log.e("", "onAttach: class has not implemented fragAhandler" );
                }
            }
            //    THIS SHOULD NEVER BE AN APPROCH.
            //    public Frag_a(FragBHandler fragBHandler) {
            //        this.fragBHandler = fragBHandler;
            //    }
    
    
            @Override
            public View onCreateView(LayoutInflater inflater, ViewGroup container,
                                     Bundle savedInstanceState) {
                // Inflate the layout for this fragment
                View v = inflater.inflate(R.layout.fragment_frag_a, container, false);
                v.findViewById(R.id.bt_open_frag2).setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        openFrag2();
                    }
                });
                v.findViewById(R.id.bt_close_frag2).setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        closeFrag2();
                    }
                });
    
                return v;
            }
    
            public void closeFrag2() {
                fragBHandler.closeFrag2();
    
            }
            public void openFrag2(){
                fragBHandler.addFrag2();
    
            }
    
        }





VOILA ,BOTH FRAGMENTS CAN LITERALLY TALK NOW!

## Fragment redemption
this was one of the most difficult task.up till now, my aim to create an app with 2 fragments communicating was complete .But when i rotated my phone, there was a  major fail: whatever that was present on the screen was being created multiple times! ![check here](https://i.stack.imgur.com/03ocn.jpg)

I found the solution by doing  things:  

 - using one common object for both frag1 and 2 that are created during onCreate(), and that too by checking if it is already present in the back-stack, then using  that object only to add again
 - adding the initial fragment also after checking the back-stack.  

So my final code becomes:




    public class MainActivity extends AppCompatActivity implements FragAHandler, FragBHandler {
        LinearLayout layRoot;
        FragmentManager manager = getSupportFragmentManager();


        Frag_a fragmentObjA;
        Frag_b fragmentObjB;
        public static final String FRAG_A_TAG = "FRAG_A";
        public static final String FRAG_B_TAG = "FRAG_B";


        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            initialise();

            if (manager.findFragmentByTag(FRAG_A_TAG) == null && manager.findFragmentByTag(FRAG_B_TAG) == null) {
                //both are imp because of testcase not giving exact answer:
                // >>>open app: shows frag1>'press open frag2'>>>frg2 opens >'close frag 1' >>> rotate.
                //Expected output >> "fragment 2(already present object in backstack and on screen) to remain on screen
                //output recieved : both frag 1 and frg2 showing
                manager.beginTransaction().add(R.id.layout_root, fragmentObjA, FRAG_A_TAG)
                        .setTransitionStyle(FragmentTransaction.TRANSIT_ENTER_MASK)
                        .commit();
            } else {
                Toast.makeText(MainActivity.this, "frag1 Already present or fragment 2 present", Toast.LENGTH_SHORT).show();
            }

        }

        private void initialise() {
            layRoot = findViewById(R.id.layout_root);

            if(manager.findFragmentByTag(FRAG_A_TAG)!=null){
                fragmentObjA= (Frag_a) manager.findFragmentByTag(FRAG_A_TAG);
                Log.e(">>", "initialise: frgement A object recieved from the frag manager is used" );

            }
            else{
                Log.e(">>", "initialise:new frag a object created" );
                fragmentObjA=new Frag_a();

            }
            if(manager.findFragmentByTag(FRAG_B_TAG)!=null){
                fragmentObjB= (Frag_b) manager.findFragmentByTag(FRAG_B_TAG);

                Log.e(">>", "initialise: frgement B object recieved from the frag manager is used" );
            }
            else{
                Log.e(">>", "initialise:new frag object created" );
                fragmentObjB=new Frag_b();
            }
        }


        //-----------handler methods-------------------
        @Override
        public void addFrag1() {
            if (manager.findFragmentByTag(FRAG_A_TAG) == null) {
                manager.beginTransaction().add(R.id.layout_root, fragmentObjA, FRAG_A_TAG)
                        .setTransitionStyle(FragmentTransaction.TRANSIT_ENTER_MASK)
                        .commit();
            } else {
                Toast.makeText(MainActivity.this, "frag1 Already present", Toast.LENGTH_SHORT).show();
            }
        }

        @Override
        public void closeFrag1() {
            Frag_a fragA = (Frag_a) manager.findFragmentByTag(FRAG_A_TAG);
            if (fragA != null) {
                manager.beginTransaction().remove(fragA)
                        .setTransitionStyle(FragmentTransaction.TRANSIT_EXIT_MASK)
                        .commit();
            } else {
                Toast.makeText(MainActivity.this, "frag1 Already not there", Toast.LENGTH_SHORT).show();

            }

        }

        @Override
        public void addFrag2() {

            if (manager.findFragmentByTag(FRAG_B_TAG) == null) {
                manager.beginTransaction().add(R.id.layout_root, fragmentObjB, FRAG_B_TAG)
                        .setTransitionStyle(FragmentTransaction.TRANSIT_ENTER_MASK)
                        .commit();
            } else {
                Toast.makeText(MainActivity.this, "frag2 Already present", Toast.LENGTH_SHORT).show();
            }


        }

        @Override
        public void closeFrag2() {
            Frag_b frag_b = (Frag_b) manager.findFragmentByTag(FRAG_B_TAG);

            if (frag_b != null) {
                manager.beginTransaction().remove(frag_b)
                        .setTransitionStyle(FragmentTransaction.TRANSIT_EXIT_MASK)
                        .commit();
            } else {
                Toast.makeText(MainActivity.this, "frag2 Already not there", Toast.LENGTH_SHORT).show();

            }

        }
        
    }
  
