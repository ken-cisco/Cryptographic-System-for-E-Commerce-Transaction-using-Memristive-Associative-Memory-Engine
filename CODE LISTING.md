PROGRAM LISTING
ECTS- OSMARS Code listings
Super admin Login code
<?php
error_reporting(0);
session_start();
include ('db.php');
$msg = '';
if($_SERVER['REQUEST_METHOD'] === 'POST'){	
		$username = mysqli_real_escape_string($link,trim($_POST['username']));
		$pass = mysqli_real_escape_string($link,$_POST['password']);
		$stat = 1;
			if($username == ''|| $pass == ''){
			$msg = 'Please complete the empty fields';
			//header('Location: login.php');
			}
			else{
			$pass = md5($pass);
				$sql = "SELECT * FROM superadminlogin WHERE uname = '$username' AND pass = '$pass' LIMIT 1";
				$query = mysqli_query($link,$sql) or die(mysqli_error($link));
		
				$rowValue = mysqli_fetch_assoc($query);
				$rowNumber = mysqli_num_rows($query);
				
				if($rowNumber == 1){
				//$_SESSION['counts']++;
			//$msg = 'User Authenticated';
			//$msg  = $rowValue['id'];
			//sessioning used
			$_SESSION['key'] = $rowValue['keyyed'];
			$_SESSION['uname'] = $rowValue['uname'];
			$otpcode = generateRandomString(6);
			$sql = "UPDATE superadminlogin SET otp = '$otpcode' WHERE uname = '$username' AND pass = '$pass' LIMIT 1";
			$query = mysqli_query($link,$sql) or die(mysqli_error($link));
			header('Location: otpsuper.php');
			}else {
			$msg = 'Invalid username or password';
					//header('Location: login.php');
			//header('Location: reg.php');
			}
			}
			}
			function generateRandomString($length = 10) {
   		 	$characters= '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
    		$charactersLength = strlen($characters);
    		$randomString = '';
    			for ($i = 0; $i < $length; $i++) {
        			$randomString .= $characters[rand(0, $charactersLength - 1)];
    			}
    			return $randomString;
			}
?>

Approval page code
<div class="listing-area">
<?php 	  
$dates = $_GET['dates'];
$emails = $_GET['emails'];
$admin = $_GET['admin'];
$approvstatus = $_GET['approvstatus'];
$sql03= "SELECT  * FROM orders WHERE email = '$emails' AND approvestatus = '$approvstatus' AND administrators = '$admin' AND date = '$dates'";
$query03 = mysqli_query($link,$sql03) or die(mysqli_error($link));
$row03 =  mysqli_fetch_assoc($query03); 
/* echo '<a href="approvalpage.php?email='.$mails.'">'.$row032['fname'].' '.$row032['lname'].' <font color = "red"><sup>'.$row03['ct'].' waiting</sup></font></a>'; *					
do{ 		  
?
                <!-- Question Listing -->
                <div class="listing-grid ">
                  <div class="row">
                    <div class="col-md-2 col-sm-2 col-xs-12 hidden-xs">
                      <a data-toggle="tooltip" data-placement="bottom" href="#"><img alt="" class=" correct img-responsive center-block" src="<?php 
					  						$product_code = $row03['product_code'];
					  						$sql03c = "SELECT  * FROM products WHERE product_code = '$product_code'";
											$query03c = mysqli_query($link,$sql03c) or die(mysqli_error($link));
											$row03c =  mysqli_fetch_assoc($query03c);
								
echo '../images/products/'.$row03c['product_img_name'];
			  						
					  				  					?>">
                      </a>
                      <span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>
                    </div>
                    <div class="col-md-7 col-sm-8  col-xs-12">
                      <h3><a  href="#"> 	
						<?php 
						  echo $row03['product_name'];
						?>
                        </a></h3>
                      <div class="listing-meta">
                        <span><i class="fa fa fa-eye" aria-hidden="true"></i><?php 
						  echo $row03['date'];
						?></span>
                      </div>
                    </div>
                     <div class="col-md-10 col-sm-10  col-xs-12">
                      <p><?php 
						  echo $row03['product_desc'];
						?></p>
                      
                    </div>

                  </div>
                </div>
                <?php 
				
				} while($row03 =  mysqli_fetch_assoc($query03));
				
				?>
                <!-- Pagination View More End -->

              </div>

OTP Code
<?php
error_reporting(0);
session_start();
include('db.php');
$msg = '';
if($_SERVER['REQUEST_METHOD'] === 'POST'){	

		$otp = mysqli_real_escape_string($link,trim($_POST['otp']));
		$stat = 1;
		
		if($otp == ''){
			
			$msg = 'Please complete the empty fields';
			//header('Location: login.php');
			
			}
			
			else{
				
				$pass = md5($pass);
				$sql = "SELECT * FROM superadminlogin WHERE otp = '$otp' LIMIT 1";
				$query = mysqli_query($link,$sql) or die(mysqli_error($link));
		
				$rowValue = mysqli_fetch_assoc($query);
				$rowNumber = mysqli_num_rows($query);
				
				if($rowNumber == 1){
				//$_SESSION['counts']++;
			//$msg = 'User Authenticated';
			//$msg  = $rowValue['id'];
			//sessioning used
			$_SESSION['key'] = $rowValue['keyyed'];
			$_SESSION['uname'] = $rowValue['uname'];
			header('Location: supercontrolp.php');
			
			}else {
				
					$msg = 'Invalid username or password';
					//header('Location: login.php');
				
				//header('Location: reg.php');
			}
						
				}
		
				}
?>

Assigned Customer Page Code
<div class="widget">
                <div class="latest-news">
                  <h2>List Of Customers Assigned To <font color="#993300"><?php echo $_GET['admin']; ?> </font></h2>
                  <hr class="widget-separator no-margin">

					<?php 
						/* $status = 1;
						$uname = $_SESSION['uname'];
						$sql03= "SELECT DISTINCT email FROM orders WHERE administrators = '$uname'";
						$query03 = mysqli_query($link,$sql03) or die(mysqli_error($link));
						$row03 =  mysqli_fetch_assoc($query03); */
					?>
                    
                    <?php 
						$adminis = $_GET['admin'];
					  	$sql032 = "SELECT * FROM users WHERE administrator = '$adminis'";
					  	$query032 = mysqli_query($link,$sql032) or die(mysqli_error($link));
					  	$row032 =  mysqli_fetch_assoc($query032);
					  
					?>
						<?php do{ ?>
                  <div class="news-post">
                    <div class="post">
                      <h5> <a href="itemspayment.php?admin=<?php echo $_GET['admin']; ?>&email=<?php echo $row032['email']; ?>">
                      <?php echo $row032['fname'].' '.$row032['lname']; ?>
                      &nbsp; - View Payments</h5></a>
                      
                    </div>

                  </div>
					<?php } while($row032 =  mysqli_fetch_assoc($query032));?>
                </div>
              </div>
            </div>
Admin Login Code
<?php
error_reporting(0);
session_start();
include('db.php');
$msg = '';
if($_SERVER['REQUEST_METHOD'] === 'POST'){	

		$username = mysqli_real_escape_string($link,trim($_POST['username']));
		$pass = mysqli_real_escape_string($link,$_POST['password']);
		$stat = 1;
		
		if($username == ''|| $pass == ''){
			
			$msg = 'Please complete the empty fields';
			//header('Location: login.php');
			
			}
			
			else{
				
				$pass = md5($pass);
				$sql = "SELECT * FROM adminlogin WHERE uname = '$username' AND pass = '$pass' LIMIT 1";
				$query = mysqli_query($link,$sql) or die(mysqli_error($link));
		
				$rowValue = mysqli_fetch_assoc($query);
				$rowNumber = mysqli_num_rows($query);
				
				if($rowNumber == 1){
				//$_SESSION['counts']++;
			//$msg = 'User Authenticated';
			//$msg  = $rowValue['id'];
			//sessioning used
			$_SESSION['key'] = $rowValue['keyyed'];
			$_SESSION['uname'] = $rowValue['uname'];
			
			$otpcode = generateRandomString(6);
			
			$sql = "UPDATE adminlogin SET otp = '$otpcode' WHERE uname = '$username' AND pass = '$pass' LIMIT 1";
				$query = mysqli_query($link,$sql) or die(mysqli_error($link));
			
			header('Location: otp.php');
			
			}else {
					$msg = 'Invalid username or password';
					//header('Location: login.php');
				
				//header('Location: reg.php');
			}
								}
			
		}
		
		function generateRandomString($length = 10) {
    $characters = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
    $charactersLength = strlen($characters);
    $randomString = '';
    for ($i = 0; $i < $length; $i++) {
        $randomString .= $characters[rand(0, $charactersLength - 1)];
    }
    return $randomString;
}
?>



Register Page
<?php
require('admin/db.php');
//if (session_status() !== PHP_SESSION_ACTIVE) {session_start();}
if(session_id() == '' || !isset($_SESSION)){session_start();}

if (isset($_SESSION["username"])) {header ("location:index.php");}
?>
<!doctype html>
<html class="no-js" lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Register || Cryptographic Analytics E-Commerce Platform</title>
    <link rel="stylesheet" href="css/foundation.css" />
    <script src="js/vendor/modernizr.js"></script>
  </head>
  <body>
    <nav class="top-bar" data-topbar role="navigation">
      <ul class="title-area">
        <li class="name">
          <h1><a href="index.php">Cryptographic Analytics E-Commerce Platform</a></h1>
        </li>
        <li class="toggle-topbar menu-icon"><a href="#"><span></span></a></li>
      </ul>
      <section class="top-bar-section">
      <!-- Right Nav Section -->
        <ul class="right">
          <li><a href="about.php">About</a></li>
          <li><a href="products.php">Products</a></li>
          <li><a href="cart.php">View Cart</a></li>
          <li><a href="orders.php">My Orders</a></li>
          <li><a href="contact.php">Contact</a></li>
          <?php
          if(isset($_SESSION['username'])){
            echo '<li><a href="account.php">My Account</a></li>';
            echo '<li><a href="logout.php">Log Out</a></li>';
          }
          else{
            echo '<li><a href="login.php">Log In</a></li>';
            echo '<li class="active"><a href="register.php">Register</a></li>';
          }
          ?>
        </ul>
      </section>
    </nav>
    <form method="POST" action="insert.php" style="margin-top:30px;">
      <div class="row">
        <div class="small-8">
          <div class="row">
            <div class="small-4 columns">
              <label for="right-label" class="right inline">First Name</label>
            </div>
            <div class="small-8 columns">
              <input type="text" id="right-label" placeholder="First Name" name="fname">
            </div>
          </div>
          <div class="row">
            <div class="small-4 columns">
              <label for="right-label" class="right inline">Last Name</label>
            </div>
            <div class="small-8 columns">
              <input type="text" id="right-label" placeholder="Last Name" name="lname">
            </div>
          </div>
          <div class="row">
            <div class="small-4 columns">
              <label for="right-label" class="right inline">Address</label>
            </div>
            <div class="small-8 columns">
              <input type="text" id="right-label" placeholder="Address" name="address">
            </div>
          </div>
          <div class="row">
            <div class="small-4 columns">
              <label for="right-label" class="right inline">City</label>
            </div>
            <div class="small-8 columns">
              <input type="text" id="right-label" placeholder="City" name="city">
            </div>
          </div>
          <div class="row">
            <div class="small-4 columns">
              <label for="right-label" class="right inline">Pin Code</label>
            </div>
            <div class="small-8 columns">
              <input type="number" id="right-label" placeholder="Pin Code" name="pin">
            </div>
          </div>
          <div class="row">
            <div class="small-4 columns">
              <label for="right-label" class="right inline">E-Mail</label>
            </div>
            <div class="small-8 columns">
              <input type="email" id="right-label" placeholder="E-Mail" name="email">
            </div>
          </div>
          <div class="row">
            <div class="small-4 columns">
              <label for="right-label" class="right inline">Password</label>
            </div>
            <div class="small-8 columns">
              <input type="password" id="right-label" name="pwd">
            </div>
          </div>
           <div class="row">
            <div class="small-4 columns">
              <label for="right-label" class="right inline">Select Administrator</label>
            </div>
            <?php 
				$status = 1;
				$sql03= "SELECT * FROM adminlogin WHERE status = '$status'";
				$query03 = mysqli_query($link,$sql03) or die(mysqli_error($link));
				$row03 =  mysqli_fetch_assoc($query03);
			?>
            <div class="small-8 columns">
              <select name="selectadmin">
              	<option value="">SELECT ADMIN</option>
                <?php do{ ?>
                <option value="<?php echo  $row03['uname']; ?>"><?php echo  $row03['uname']; ?></option>
                <?php } while($row03 =  mysqli_fetch_assoc($query03));?>
              </select>
            </div>
          </div>
            <div class="row">
            <div class="small-4 columns">
            </div>
            <div class="small-8 columns">
              <input type="submit" id="right-label" value="Register" style="background: #0078A0; border: none; color: #fff; font-family: 'Helvetica Neue', sans-serif; font-size: 1em; padding: 10px;">
              <input type="reset" id="right-label" value="Reset" style="background: #0078A0; border: none; color: #fff; font-family: 'Helvetica Neue', sans-serif; font-size: 1em; padding: 10px;">
            </div>
          </div>
        </div>
      </div>
    </form>
    <div class="row" style="margin-top:10px;">
      <div class="small-12">
        <footer>
           <p style="text-align:center; font-size:0.8em;">&copy; Cryptographic Analytics E-Commerce Platform. All Rights Reserved.</p>
        </footer>
      </div>
    </div>
   <script src="js/vendor/jquery.js"></script>
    <script src="js/foundation.min.js"></script>
    <script>
      $(document).foundation();
    </script>
  </body>
</html>

Order Page
<?php

//if (session_status() !== PHP_SESSION_ACTIVE) {session_start();}
if(session_id() == '' || !isset($_SESSION)){session_start();}

if(!isset($_SESSION["username"])){
  header("location:index.php");
}
include 'config.php';
?>
<!doctype html>
<html class="no-js" lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Orders || Cryptographic Analytics E-Commerce Platform</title>
    <link rel="stylesheet" href="css/foundation.css" />
    <script src="js/vendor/modernizr.js"></script>
  </head>
  <body>
    <nav class="top-bar" data-topbar role="navigation">
      <ul class="title-area">
        <li class="name">
          <h1><a href="index.php">Cryptographic Analytics E-Commerce Platform</a></h1>
        </li>
        <li class="toggle-topbar menu-icon"><a href="#"><span></span></a></li>
      </ul>
      <section class="top-bar-section">
      <!-- Right Nav Section -->
        <ul class="right">
          <li><a href="about.php">About</a></li>
          <li><a href="products.php">Products</a></li>
          <li><a href="cart.php">View Cart</a></li>
          <li class="active"><a href="orders.php">My Orders</a></li>
          <li><a href="contact.php">Contact</a></li>
          <?php
          if(isset($_SESSION['username'])){
            echo '<li><a href="account.php">My Account</a></li>';
            echo '<li><a href="logout.php">Log Out</a></li>';
          }
          else{
            echo '<li><a href="login.php">Log In</a></li>';
            echo '<li><a href="register.php">Register</a></li>';
          }
          ?>
        </ul>
      </section>
    </nav>
    <div class="row" style="margin-top:10px;">
      <div class="large-12">
        <h3>My COD Orders</h3>
        <hr>
        <?php
          $user = $_SESSION["username"];
          $result = $mysqli->query("SELECT * from orders where email='".$user."'");
          if($result) {
            while($obj = $result->fetch_object()) {
              //echo '<div class="large-6">';
              echo '<p><h4>Order ID ->'.$obj->id.'</h4></p>';
              echo '<p><strong>Date of Purchase</strong>: '.$obj->date.'</p>';
              echo '<p><strong>Product Code</strong>: '.$obj->product_code.'</p>';
              echo '<p><strong>Product Name</strong>: '.$obj->product_name.'</p>';
              echo '<p><strong>Price Per Unit</strong>: '.$obj->price.'</p>';
              echo '<p><strong>Units Bought</strong>: '.$obj->units.'</p>';
              echo '<p><strong>Total Cost</strong>: '.$currency.$obj->total.'</p>';
              //echo '</div>';
              //echo '<div class="large-6">';
              //echo '<img src="images/products/sports_band.jpg">';
              //echo '</div>';
              echo '<p><hr></p>';
            }
          }
        ?>
      </div>
    </div>
    <div class="row" style="margin-top:10px;">
      <div class="small-12">
        <footer style="margin-top:10px;">
           <p style="text-align:center; font-size:0.8em;">&copy; Cryptographic Analytics E-Commerce Platform. All Rights Reserved.</p>
        </footer>
      </div>
    </div>
    <script src="js/vendor/jquery.js"></script>
    <script src="js/foundation.min.js"></script>
    <script>
      $(document).foundation();
    </script>
  </body>
</html>


Products Page
<?php
//if (session_status() !== PHP_SESSION_ACTIVE) {session_start();}
if(session_id() == '' || !isset($_SESSION)){session_start();}
include 'config.php';
?>
<!doctype html>
<html class="no-js" lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Products || Cryptographic Analytics E-Commerce Platform</title>
    <link rel="stylesheet" href="css/foundation.css" />
    <script src="js/vendor/modernizr.js"></script>
  </head>
  <body>
    <nav class="top-bar" data-topbar role="navigation">
      <ul class="title-area">
        <li class="name">
          <h1><a href="index.php">Cryptographic Analytics E-Commerce Platform</a></h1>
        </li>
        <li class="toggle-topbar menu-icon"><a href="#"><span></span></a></li>
      </ul>
      <section class="top-bar-section">
      <!-- Right Nav Section -->
        <ul class="right">
          <li><a href="about.php">About</a></li>
          <li class='active'><a href="products.php">Products</a></li>
          <li><a href="cart.php">View Cart</a></li>
          <li><a href="orders.php">My Orders</a></li>
          <li><a href="contact.php">Contact</a></li>
          <?php
          if(isset($_SESSION['username'])){
            echo '<li><a href="account.php">My Account</a></li>';
            echo '<li><a href="logout.php">Log Out</a></li>';
          }
          else{
            echo '<li><a href="login.php">Log In</a></li>';
            echo '<li><a href="register.php">Register</a></li>';
          }
          ?>
        </ul>
      </section>
    </nav>
    <div class="row" style="margin-top:10px;">
      <div class="small-12">
        <?php
          $i=0;
          $product_id = array();
          $product_quantity = array();
          $result = $mysqli->query('SELECT * FROM products');
          if($result === FALSE){
            die(mysql_error());
          }
          if($result){
            while($obj = $result->fetch_object()) {
              echo '<div class="large-4 columns">';
              echo '<p><h3>'.$obj->product_name.'</h3></p>';
              echo '<img src="images/products/'.$obj->product_img_name.'"/>';
              echo '<p><strong>Product Code</strong>: '.$obj->product_code.'</p>';
              echo '<p><strong>Description</strong>: '.$obj->product_desc.'</p>';
              echo '<p><strong>Units Available</strong>: '.$obj->qty.'</p>';
              echo '<p><strong>Price (Per Unit)</strong>: &#8358;'.$obj->price.'</p>';
              if($obj->qty > 0){
                echo '<p><a href="update-cart.php?action=add&id='.$obj->id.'"><input type="submit" value="Add To Cart" style="clear:both; background: #0078A0; border: none; color: #fff; font-size: 1em; padding: 10px;" /></a></p>';
              }
              else {
                echo 'Out Of Stock!';
              }
              echo '</div>';

              $i++;
            }

          }
          $_SESSION['product_id'] = $product_id;
          echo '</div>';
          echo '</div>';
          ?>
        <div class="row" style="margin-top:10px;">
          <div class="small-12">
        <footer style="margin-top:10px;">
           <p style="text-align:center; font-size:0.8em;clear:both;">&copy; Cryptographic Analytics E-Commerce Platform. All Rights Reserved.</p>
        </footer>
      </div>
    </div>
    <script src="js/vendor/jquery.js"></script>
    <script src="js/foundation.min.js"></script>
    <script>
      $(document).foundation();
    </script>
  </body>
</html>

Make payment Page
<?php
//if (session_status() !== PHP_SESSION_ACTIVE) {session_start();}
if(session_id() == '' || !isset($_SESSION)){session_start();}
if(!isset($_SESSION["username"])){
  header("location:index.php");
}
include 'config.php';
?>
<!doctype html>
<html class="no-js" lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Orders || Cryptographic Analytics E-Commerce Platform</title>
    <link rel="stylesheet" href="css/foundation.css" />
    <script src="js/vendor/modernizr.js"></script>
  </head>
  <body>
    <nav class="top-bar" data-topbar role="navigation">
      <ul class="title-area">
        <li class="name">
          <h1><a href="index.php">Cryptographic Analytics E-Commerce Platform</a></h1>
        </li>
        <li class="toggle-topbar menu-icon"><a href="#"><span></span></a></li>
      </ul>
      <section class="top-bar-section">
      <!-- Right Nav Section -->
        <ul class="right">
          <li><a href="about.php">About</a></li>
          <li><a href="products.php">Products</a></li>
          <li><a href="cart.php">View Cart</a></li>
          <li class="active"><a href="orders.php">My Orders</a></li>
          <li><a href="contact.php">Contact</a></li>
          <?php
          if(isset($_SESSION['username'])){
            echo '<li><a href="account.php">My Account</a></li>';
            echo '<li><a href="logout.php">Log Out</a></li>';
          }
          else{
            echo '<li><a href="login.php">Log In</a></li>';
            echo '<li><a href="register.php">Register</a></li>';
          }
          ?>
        </ul>
      </section>
    </nav>
    <div class="row" style="margin-top:10px;">
      <div class="large-12">
        <h3>My COD Orders</h3>
        <hr>

        <?php
          $user = $_SESSION["username"];
		  $status = 1;
		  $paymentstatus = 0;
          $result = $mysqli->query("SELECT * from orders where email='".$user."' AND approvestatus='".$status."' AND paymentstatus='".$paymentstatus."'");
          if($result) {
            while($obj = $result->fetch_object()) {
              //echo '<div class="large-6">';
              echo '<p><h4>Order ID ->'.$obj->id.'</h4></p>';
              echo '<p><strong>Date of Purchase</strong>: '.$obj->date.'</p>';
              echo '<p><strong>Product Code</strong>: '.$obj->product_name.'</p>';
			  echo '<p><strong>Unit Price</strong>: '.$obj->price.'</p>';
			  echo '<p><strong>Quantity</strong>: '.$obj->units.'</p>';
			  echo '<p><strong>Total</strong>: '.$obj->total.'</p>';
			  
			  echo '<p><a href="makepaymentsucces.php?dates='.$obj->date.'&admin='.$obj->administrators.'"><strong>Pay</strong></a> </p>';
              //echo '</div>';
              //echo '<div class="large-6">';
              //echo '<img src="images/products/sports_band.jpg">';
              //echo '</div>';
              echo '<p><hr></p>';
	            }
          }
        ?>
      </div>
    </div>
    <div class="row" style="margin-top:10px;">
      <div class="small-12">
        <footer style="margin-top:10px;">
           <p style="text-align:center; font-size:0.8em;">&copy; Cryptographic Analytics E-Commerce Platform. All Rights Reserved.</p>
        </footer>
      </div>
    </div>
    <script src="js/vendor/jquery.js"></script>
    <script src="js/foundation.min.js"></script>
    <script>
      $(document).foundation();
    </script>
  </body>
</html>

Shopping cart page
<?php
//if (session_status() !== PHP_SESSION_ACTIVE) {session_start();}
if(session_id() == '' || !isset($_SESSION)){session_start();}
include 'config.php';
?>
<!doctype html>
<html class="no-js" lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Shopping Cart || Cryptographic Analytics E-Commerce Platform</title>
    <link rel="stylesheet" href="css/foundation.css" />
    <script src="js/vendor/modernizr.js"></script>
  </head>
  <body>
    <nav class="top-bar" data-topbar role="navigation">
      <ul class="title-area">
        <li class="name">
          <h1><a href="index.php">Cryptographic Analytics E-Commerce Platform</a></h1>
        </li>
        <li class="toggle-topbar menu-icon"><a href="#"><span></span></a></li>
      </ul>
      <section class="top-bar-section">
      <!-- Right Nav Section -->
        <ul class="right">
          <li><a href="about.php">About</a></li>
          <li><a href="products.php">Products</a></li>
          <li class="active"><a href="cart.php">View Cart</a></li>
          <li><a href="orders.php">My Orders</a></li>
          <li><a href="contact.php">Contact</a></li>
          <?php

          if(isset($_SESSION['username'])){
            echo '<li><a href="account.php">My Account</a></li>';
            echo '<li><a href="logout.php">Log Out</a></li>';
          }
          else{
            echo '<li><a href="login.php">Log In</a></li>';
            echo '<li><a href="register.php">Register</a></li>';
          }
          ?>
        </ul>
      </section>
    </nav>
    <div class="row" style="margin-top:10px;">
      <div class="large-12">
        <?php

          echo '<p><h3>Your Shopping Cart</h3></p>';

          if(isset($_SESSION['cart'])) {

            $total = 0;
            echo '<table>';
            echo '<tr>';
            echo '<th>Code</th>';
            echo '<th>Name</th>';
            echo '<th>Quantity</th>';
            echo '<th>Cost</th>';
            echo '</tr>';
            foreach($_SESSION['cart'] as $product_id => $quantity) {
            $result = $mysqli->query("SELECT product_code, product_name, product_desc, qty, price FROM products WHERE id = ".$product_id);

            if($result){

              while($obj = $result->fetch_object()) {
                $cost = $obj->price * $quantity; //work out the line cost
                $total = $total + $cost; //add to the total cost

                echo '<tr>';
                echo '<td>'.$obj->product_code.'</td>';
                echo '<td>'.$obj->product_name.'</td>';
                echo '<td>'.$quantity.'&nbsp;<a class="button [secondary success alert]" style="padding:5px;" href="update-cart.php?action=add&id='.$product_id.'">+</a>&nbsp;<a class="button alert" style="padding:5px;" href="update-cart.php?action=remove&id='.$product_id.'">-</a></td>';
                echo '<td>'.$cost.'</td>';
                echo '</tr>';
              }
            }

          }
          echo '<tr>';
          echo '<td colspan="3" align="right">Total</td>';
          echo '<td>'.$total.'</td>';
          echo '</tr>';
          echo '<tr>';
          echo '<td colspan="4" align="right"><a href="update-cart.php?action=empty" class="button alert">Empty Cart</a>&nbsp;<a href="products.php" class="button [secondary success alert]">Continue Shopping</a>';
          if(isset($_SESSION['username'])) {
            echo '<a href="orders-update.php"><button style="float:right;">Submit For Approval</button></a>';
          }
          else {
            echo '<a href="login.php"><button style="float:right;">Login</button></a>';
          }
          echo '</td>';
          echo '</tr>';
          echo '</table>';
        }
        else {
          echo "You have no items in your shopping cart.";
        }
          echo '</div>';
          echo '</div>';
          ?>
    <div class="row" style="margin-top:10px;">
      <div class="small-12">
        <footer style="margin-top:10px;">
           <p style="text-align:center; font-size:0.8em;clear:both;">&copy; Cryptographic Analytics E-Commerce Platform. All Rights Reserved.</p>
        </footer>
      </div>
    </div>
    <script src="js/vendor/jquery.js"></script>
    <script src="js/foundation.min.js"></script>
    <script>
      $(document).foundation();
    </script>
  </body>
</html>

Customer Login
<?php

//if (session_status() !== PHP_SESSION_ACTIVE) {session_start();}
if(session_id() == '' || !isset($_SESSION)){session_start();}

if(isset($_SESSION["username"])){

        header("location:index.php");
}
?>
<!doctype html>
<html class="no-js" lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Login || Cryptographic Analytics E-Commerce Platform</title>
    <link rel="stylesheet" href="css/foundation.css" />
    <script src="js/vendor/modernizr.js"></script>
  </head>
  <body>
    <nav class="top-bar" data-topbar role="navigation">
      <ul class="title-area">
        <li class="name">
          <h1><a href="index.php">Cryptographic Analytics E-Commerce Platform</a></h1>
        </li>
        <li class="toggle-topbar menu-icon"><a href="#"><span></span></a></li>
      </ul>
      <section class="top-bar-section">
      <!-- Right Nav Section -->
        <ul class="right">
          <li><a href="about.php">About</a></li>
          <li><a href="products.php">Products</a></li>
          <li><a href="cart.php">View Cart</a></li>
          <li><a href="orders.php">My Orders</a></li>
          <li><a href="contact.php">Contact</a></li>
          <?php
          if(isset($_SESSION['username'])){
            echo '<li><a href="account.php">My Account</a></li>';
            echo '<li><a href="logout.php">Log Out</a></li>';
          }
          else{
            echo '<li class="active"><a href="login.php">Log In</a></li>';
            echo '<li><a href="register.php">Register</a></li>';
          }
          ?>
        </ul>
      </section>
    </nav>
    <form method="POST" action="verify.php" style="margin-top:30px;">
      <div class="row">
        <div class="small-8">
          <div class="row">
            <div class="small-4 columns">
              <label for="right-label" class="right inline">Email</label>
            </div>
            <div class="small-8 columns">
              <input type="email" id="right-label" placeholder="nayantronix@gmail.com" name="username">
            </div>
          </div>
          <div class="row">
            <div class="small-4 columns">
              <label for="right-label" class="right inline">Password</label>
            </div>
            <div class="small-8 columns">
              <input type="password" id="right-label" name="pwd">
            </div>
          </div>
          <div class="row">
            <div class="small-4 columns">
            </div>
            <div class="small-8 columns">
              <input type="submit" id="right-label" value="Login" style="background: #0078A0; border: none; color: #fff; font-family: 'Helvetica Neue', sans-serif; font-size: 1em; padding: 10px;">
              <input type="reset" id="right-label" value="Reset" style="background: #0078A0; border: none; color: #fff; font-family: 'Helvetica Neue', sans-serif; font-size: 1em; padding: 10px;">
            </div>
          </div>
        </div>
      </div>
    </form>
    <div class="row" style="margin-top:10px;">
      <div class="small-12">
        <footer>
           <p style="text-align:center; font-size:0.8em;">&copy; Cryptographic Analytics E-Commerce Platform. All Rights Reserved.</p>
        </footer>
      </div>
    </div>
    <script src="js/vendor/jquery.js"></script>
    <script src="js/foundation.min.js"></script>
    <script>
      $(document).foundation();
    </script>
  </body>
</html>

