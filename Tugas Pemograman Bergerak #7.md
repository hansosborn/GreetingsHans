# GreetingsHans
Tugas Pemograman Bergerak

XML CODE
<ScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context=".MainActivity" >

        <EditText
            android:id="@+id/name_field"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:inputType="textCapWords"
            android:hint="Name"
            android:layout_margin="16dp" />

        <CheckBox
            android:id="@+id/whipped_cream_checkbox"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Whipped Cream"
            android:textSize="16sp"
            android:layout_marginLeft="16dp"
            android:paddingLeft="16dp" />

        <CheckBox
            android:id="@+id/chocolate_checkbox"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Chocolate"
            android:textSize="16sp"
            android:layout_marginLeft="16dp"
            android:paddingLeft="16dp"
            />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="QUANTITY"
            android:textColor="@android:color/darker_gray"
            android:padding="16dp"/>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <Button
                android:layout_width="48dp"
                android:layout_height="48dp"
                android:text="-"
                android:layout_marginLeft="16dp"
                android:layout_marginRight="8dp"
                android:onClick="Decrement" />

            <TextView
                android:id="@+id/quantity_text_view"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:textColor="@android:color/black"
                android:text="2"
                android:padding="8dp"
                android:textSize="16sp" />

            <Button
                android:layout_width="48dp"
                android:layout_height="48dp"
                android:text="+"
                android:layout_marginLeft="8dp"
                android:layout_marginRight="8dp"
                android:onClick="Increment" />

        </LinearLayout>


        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textAllCaps="true"
            android:text="order summary"
            android:textColor="@android:color/darker_gray"
            android:paddingTop="16dp"
            android:paddingLeft="16dp"/>

        <TextView
            android:id="@+id/order_summary_text_view"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="@android:color/black"
            android:text="$10.00"
            android:padding="16dp"
            android:textSize="16sp"
            />

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Order"
            android:layout_marginLeft="16dp"
            android:onClick="submitOrder"/>

    </LinearLayout>
</ScrollView>



JAVA CODE
package com.example.justjava;


import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import android.view.View;
import android.widget.CheckBox;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;


/**
 * This app displays an order form to order coffee.
 */
public class MainActivity extends AppCompatActivity {

    int quantity = 2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    /**
     * Public void adalah method dan akan menghasilkan 1 apabila ditekan order
     Call the method
     calling method
     */
    public void submitOrder(View view) {
        EditText nameField = (EditText) findViewById(R.id.name_field);
        String value = nameField.getText().toString();

        CheckBox whippedCreamCheckbox = (CheckBox) findViewById(R.id.whipped_cream_checkbox);
        boolean hasWhippedCream = whippedCreamCheckbox.isChecked();

        CheckBox chocolateCheckBox = (CheckBox) findViewById(R.id.chocolate_checkbox);
        boolean hasChocolate = chocolateCheckBox.isChecked();

        int price = calculatePrice(hasWhippedCream, hasChocolate);
        String priceMessage = createOrderSummary(price, hasWhippedCream, hasChocolate);

        Intent intent = new Intent(Intent.ACTION_SENDTO);
        intent.setData(Uri.parse("mailto:")); //agar hanya aplikasi email yg handle ini
        intent.putExtra(Intent.EXTRA_SUBJECT,"Just Java Order for" + nameField);
        intent.putExtra(Intent.EXTRA_TEXT, priceMessage);
        if (intent.resolveActivity(getPackageManager())!=null) {
            startActivity(intent);
        }

    }
    /*
    create summary of order
    @param price of the order
    @param addWhippedCream is whether or not the user wants whipped cream topping
    return text Summary */

    private String createOrderSummary (int price, boolean addWhippedCream, boolean addChocolate) {
                String priceMessage = "Name : Hans Alghifay";
                priceMessage += "\nAdd Whipped Cream? " + addWhippedCream;
                priceMessage += "\nAdd Chocolate? " + addChocolate;
                priceMessage += "\nQuantity: " + quantity;
                priceMessage += "\nTotal: $" + price;
                priceMessage += "\nThank You";
                return priceMessage;
    }
    /**
     * Calculates the price of the order.
     * @return total price
     **/
    private int calculatePrice(boolean addWhippedCream, boolean addChocolate) {
        int basePrice = 5;

        if (addWhippedCream) {
            basePrice = basePrice + 1;
        }
        if (addChocolate) {
            basePrice = basePrice + 2;
        }

        return quantity * basePrice;
    }


    //nambah kuantiti
    //pemanggilan method
    public void Increment(View view) {
        if (quantity == 100) {
            Toast.makeText(this, "MAXIMUM", Toast.LENGTH_SHORT).show();
            return;
        }
        quantity = quantity + 1;
        displayQuantity(quantity);
    }

    //kurang kuantiti
    //pemanggilan method
    public void Decrement(View view) {
        if (quantity == 1) {
            Toast.makeText(this, "MINIMUM", Toast.LENGTH_SHORT).show();
            return;
        }
        quantity = quantity - 1;
        displayQuantity(quantity);
    }


    /**
     * This method displays the given quantity value on the screen.
     * Declare the method
     * penjelasan method
     */
    private void displayQuantity (int number) {
        TextView quantityTextView = (TextView) findViewById(R.id.quantity_text_view);
        quantityTextView.setText("" + number);
    }
}
