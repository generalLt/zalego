# zalego
package com.example.lt.interview;

import android.app.ProgressDialog;
import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.text.TextUtils;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import com.android.volley.DefaultRetryPolicy;
import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;

import java.util.HashMap;
import java.util.Map;

public class login extends AppCompatActivity {
    EditText txtusername,txtpassword;
    Button login;
    String username,password;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);
        txtusername=(EditText)findViewById(R.id.username);
        txtpassword=(EditText)findViewById(R.id.password);

    }

    public void login(View view) {

        username=txtusername.getText().toString().trim();
        password=txtpassword.getText().toString().trim();
        if(TextUtils.isEmpty(username)) {
            txtusername.setError("Enter username");
        }else if(TextUtils.isEmpty(password)){
            txtpassword.setError("Enter password");
        }
        else {

            final ProgressDialog dialog=new ProgressDialog(this);
            dialog.setMessage("Logging in.....Please wait");
            dialog.setCancelable(false);
            dialog.show();

            final String url=new Url().getUrl()+ "login.php";
            StringRequest stringRequest = new StringRequest(Request.Method.POST, url,
                    new Response.Listener<String>() {
                        @Override
                        public void onResponse(String response) {
                            dialog.dismiss();
                            //Toast.makeText(Login.this, response.toString(),Toast.LENGTH_LONG).show();

                            if (response.equals("Successful")){





                               Toast.makeText(login.this,"Login successfull",Toast.LENGTH_LONG).show();
                                finish();

                            }else if (response.equals("Unsuccessful")){
                                Toast.makeText(login.this,"Incorrect Username or password",Toast.LENGTH_LONG).show();
                            }else {
                                //Toast.makeText(Login.this,"There was an unknown error",Toast.LENGTH_LONG).show();
                            }
                        }
                    },
                    new Response.ErrorListener() {
                        @Override
                        public void onErrorResponse(VolleyError error) {
                            dialog.dismiss();
                            Toast.makeText(login.this,error.toString(),Toast.LENGTH_LONG).show();
                        }
                    }){
                @Override
                protected Map<String,String> getParams(){
                    Map<String,String> params = new HashMap<String, String>();
                    params.put("username",username);
                    params.put("password",password);
                    return params;
                }

            };
            RequestQueue requestQueue = Volley.newRequestQueue(this);
            requestQueue.add(stringRequest);
            stringRequest.setRetryPolicy(new DefaultRetryPolicy(10000,
                    DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
                    DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
        }



    }

    public void logins(View view) {
        startActivity(new Intent(login.this, com.example.lt.interview.MainActivity.class));
    }
}

