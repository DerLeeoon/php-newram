# php-newram

<?php
// Initialize the session
session_start();
 
// Check if the user is logged in, if not then redirect to login page
if(!isset($_SESSION["loggedin"]) || $_SESSION["loggedin"] !== true){
    header("location: /login.php");
    exit;
}
 
// Include config file
require_once "config.php";
 
// Define variables and initialize with empty values
$new_password = $confirm_password = "";
$new_password_err = $confirm_password_err = "";
 
// Processing form data when form is submitted
if($_SERVER["REQUEST_METHOD"] == "POST"){
 
    // Validate new password
    if(empty(trim($_POST["new_password"]))){
        $new_password_err = "Please enter the new password.";     
    } elseif(strlen(trim($_POST["new_password"])) < 6){
        $new_password_err = "Password must have atleast 6 characters.";
    } else{
        $new_password = trim($_POST["new_password"]);
    }
    
    // Validate confirm password
    if(empty(trim($_POST["confirm_password"]))){
        $confirm_password_err = "Please confirm the password.";
    } else{
        $confirm_password = trim($_POST["confirm_password"]);
        if(empty($new_password_err) && ($new_password != $confirm_password)){
            $confirm_password_err = "Password did not match.";
        }
    }
        
    // Check input errors before updating the database
    if(empty($new_password_err) && empty($confirm_password_err)){
        // Prepare an update statement
        $sql = "UPDATE users SET password = ? WHERE id = ?";
        
        if($stmt = mysqli_prepare($link, $sql)){
            // Bind variables to the prepared statement as parameters
            mysqli_stmt_bind_param($stmt, "si", $param_password, $param_id);
            
            // Set parameters
            $param_password = password_hash($new_password, PASSWORD_DEFAULT);
            $param_id = $_SESSION["id"];
            
            // Attempt to execute the prepared statement
            if(mysqli_stmt_execute($stmt)){
                // Password updated successfully. Destroy the session, and redirect to login page
                session_destroy();
                header("location: /login.php");
                exit();
            } else{
                echo "Oops! Something went wrong. Please try again later.";
            }

            // Close statement
            mysqli_stmt_close($stmt);
        }
    }
    
    // Close connection
    mysqli_close($link);
}
?>

<?php
$pdo = new PDO('mysql:host=localhost;dbname=discord-bot', 'discord_bot', 'Mv2#kl61');

$sql = "SELECT Ram, Cpu, Speicher, Standort, Traffic, Backup, DDos, System, Preis FROM kvm";
foreach ($pdo->query($sql) as $row) {
}


$stat = $pdo->prepare("UPDATE kvm SET Ram = ?");
$stat->execute($new_ram);
?>

<!-- PHP Ebende Ende -->
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <meta name="author" content="Léon Kiehn">
  <title>OneKlick-Installer.de - Account bearbeiten</title>
  <!-- Favicon -->
  <link rel="icon" href="../assets/img/brand/favicon.png" type="image/png">
  <!-- Fonts -->
  <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Open+Sans:300,400,600,700">
  <!-- Icons -->
  <link rel="stylesheet" href="../assets/vendor/nucleo/css/nucleo.css" type="text/css">
  <link rel="stylesheet" href="../assets/vendor/@fortawesome/fontawesome-free/css/all.min.css" type="text/css">
  <!-- Argon CSS -->
  <link rel="stylesheet" href="../assets/css/argon.css?v=1.2.0" type="text/css">
</head>

<body>
  <!-- Sidenav -->
  <nav class="sidenav navbar navbar-vertical  fixed-left  navbar-expand-xs navbar-light bg-white" id="sidenav-main">
    <div class="scrollbar-inner">
      <!-- Brand -->
      <div class="sidenav-header  align-items-center">
        <a class="navbar-brand" href="javascript:void(0)">
          <img height="100" src="https://oneklick-installer.de/assets/img/logo.png" class="navbar-brand-img" alt="">
        </a>
      </div>
      <div class="navbar-inner">
        <!-- Collapse -->
        <div class="collapse navbar-collapse" id="sidenav-collapse-main">
          <!-- Nav items -->
          <ul class="navbar-nav">
            <li class="nav-item">
              <a class="nav-link active" href="../">
                <i class="ni ni-tv-2 text-primary"></i>
                <span class="nav-link-text">Dashboard</span>
              </a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="https://oneklick-installer.de/dashboard/oneklick-installer/order">
              <i class="far fa-file-code"></i>
                <span class="nav-link-text">OneKlick-Installer</span>
              </a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="https://wiki.xeotech.eu/doku.php?id=oneklick-installer:start">
                <i class="fas fa-info"></i>
                <span class="nav-link-text">Wiki</span>
              </a>
            </li>
            <hr class="my-3">
            <li class="nav-item">
              <a class="nav-link" href="/logout"><i class="uil uil-signout"></i>
                <i class="ni ni-send text-dark"></i>
                <span class="nav-link-text">Logout</span>
              </a>
            </li>
          </ul>
          <!-- Divider -->
          <hr class="my-3">
          <!-- Heading -->
        </div>
      </div>
    </div>
  </nav>
  <!-- Main content -->
  <div class="main-content" id="panel">
    <!-- Topnav -->
    <nav class="navbar navbar-top navbar-expand navbar-dark bg-primary border-bottom">
      <div class="container-fluid">
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <!-- Search form -->
          <form class="navbar-search navbar-search-light form-inline mr-sm-3" id="navbar-search-main">
            <div class="form-group mb-0">
              <div class="input-group input-group-alternative input-group-merge">
                <div class="input-group-prepend">
                  <span class="input-group-text"><i class="fas fa-search"></i></span>
                </div>
                <input class="form-control" placeholder="Search" type="text">
              </div>
            </div>
            <button type="button" class="close" data-action="search-close" data-target="#navbar-search-main" aria-label="Close">
              <span aria-hidden="true">×</span>
            </button>
          </form>
          <!-- Navbar links -->
          <ul class="navbar-nav align-items-center  ml-md-auto ">
            <li class="nav-item d-xl-none">
              <!-- Sidenav toggler -->
              <div class="pr-3 sidenav-toggler sidenav-toggler-dark" data-action="sidenav-pin" data-target="#sidenav-main">
                <div class="sidenav-toggler-inner">
                  <i class="sidenav-toggler-line"></i>
                  <i class="sidenav-toggler-line"></i>
                  <i class="sidenav-toggler-line"></i>
                </div>
              </div>
            </li>
            <li class="nav-item d-sm-none">
              <a class="nav-link" href="#" data-action="search-show" data-target="#navbar-search-main">
                <i class="ni ni-zoom-split-in"></i>
              </a>
            </li>
            <li class="nav-item dropdown">
              <a class="nav-link" href="#" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                <i class="ni ni-bell-55"></i>
              </a>
              <div class="dropdown-menu dropdown-menu-xl  dropdown-menu-right  py-0 overflow-hidden">
                <!-- Dropdown header -->
                <div class="px-3 py-3">
                  <h6 class="text-sm text-muted m-0">Du hast <strong class="text-primary">00</strong> Nachrichten.</h6>
                </div>
                <!-- List group -->
                <div class="list-group list-group-flush">
                  <a href="#!" class="list-group-item list-group-item-action">
                    <div class="row align-items-center">
                      <div class="col-auto">
                        <!-- Avatar -->
                        <img alt="Image placeholder" src="assets/img/theme/team-1.jpg" class="avatar rounded-circle">
                      </div>
                      <div class="col ml--2">
                        <div class="d-flex justify-content-between align-items-center">
                          <div>
                            <h4 class="mb-0 text-sm">Test</h4>
                          </div>
                          <div class="text-right text-muted">
                            <small>vor 3.Stunden</small>
                          </div>
                        </div>
                        <p class="text-sm mb-0">Warum so dumm?</p>
                      </div>
                    </div>
                  </a>
                <!-- View all -->
                <a href="#!" class="dropdown-item text-center text-primary font-weight-bold py-3">Alle ansehen</a>
              </div>
            </li>
            <li class="nav-item dropdown">
              <a class="nav-link" href="#" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                <i class="ni ni-ungroup"></i>
              </a>
              <div class="dropdown-menu dropdown-menu-lg dropdown-menu-dark bg-default  dropdown-menu-right ">
                <div class="row shortcuts px-4">
                  <a href="/zahlungen" class="col-4 shortcut-item">
                    <span class="shortcut-media avatar rounded-circle bg-gradient-info">
                      <i class="ni ni-credit-card"></i>
                    </span>
                    <small>Zahlungen</small>
                  </a>
                  <a href="/kontakt" class="col-4 shortcut-item">
                    <span class="shortcut-media avatar rounded-circle bg-gradient-green">
                      <i class="ni ni-books"></i>
                    </span>
                    <small>Support</small>
                  </a>
                  <a href="https:/cp.xeotech.eu" class="col-4 shortcut-item">
                    <span class="shortcut-media avatar rounded-circle bg-gradient-yellow">
                      <i class="ni ni-basket"></i>
                    </span>
                    <small>Shop</small>
                  </a>
                </div>
              </div>
            </li>
          </ul>
          <ul class="navbar-nav align-items-center  ml-auto ml-md-0 ">
            <li class="nav-item dropdown">
              <a class="nav-link pr-0" href="#" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                <div class="media align-items-center">
                  <span class="avatar avatar-sm rounded-circle">
                    <img alt="Image placeholder" src="https://picsum.photos/128/128">
                  </span>
                  <div class="media-body  ml-2  d-none d-lg-block">
                    <span class="mb-0 text-sm  font-weight-bold"><?php $username = $_SESSION['username'];echo "".$username;?></span>
                  </div>
                </div>
              </a>
              <div class="dropdown-menu  dropdown-menu-right ">
                <div class="dropdown-header noti-title">
                  <h6 class="text-overflow m-0">Willkommen!</h6>
                </div>
                <a href="account/edit" class="dropdown-item">
                  <i class="ni ni-single-02"></i>
                  <span>Mein Account</span>
                </a>
                <a href="/kontakt" class="dropdown-item">
                  <i class="ni ni-support-16"></i>
                  <span>Support</span>
                </a>
                <div class="dropdown-divider"></div>
                <a href="../logout" class="dropdown-item">
                  <i class="ni ni-send"></i>
                  <span>Logout</span>
                </a>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <!-- Header -->
    <!-- Header -->
    <div class="header pb-6 d-flex align-items-center" style="min-height: 500px; background-image: url(#); background-size: cover; background-position: center top;">
      <!-- Mask -->
      <span class="mask bg-gradient-default opacity-8"></span>
      <!-- Header container -->
      <div class="container-fluid d-flex align-items-center">
        <div class="row">
          <div class="col-lg-7 col-md-10">
            <h1 class="display-2 text-white"><?php $username = $_SESSION['username'];echo "Moin, ".$username;?></h1>
            <p class="text-white mt-0 mb-5">Hier kannst du Änderungen an deinem Account vohrnehmen, z.B. Passwort ändern oder Zusatztdaten hinzufügen</p>
          </div>
        </div>
      </div>
    </div>
    <!-- Page content -->
    <div class="container-fluid mt--6">
      <div class="row">
        <div class="col-xl-4 order-xl-2">
          <div class="card card-profile">
            <img src="https://picsum.photos/1000/600" alt="Image placeholder" class="card-img-top">
            <div class="row justify-content-center">
              <div class="col-lg-3 order-lg-2">
                <div class="card-profile-image">
                  <a href="#">
                    <img src="https://picsum.photos/128/128" class="rounded-circle">
                  </a>
                </div>
              </div>
            </div>
            <div class="card-header text-center border-0 pt-8 pt-md-4 pb-0 pb-md-4">
              <div class="d-flex justify-content-between">
                <a href="https://discord.com/invite/rHChvUDESh" class="btn btn-sm btn-info  mr-4 ">Discord</a>
                <a href="#" class="btn btn-sm btn-default float-right">Message</a>
              </div>
            </div>
            <div class="card-body pt-0">
              <div class="row">
                <div class="col">
                  <div class="card-profile-stats d-flex justify-content-center">
                    <div>
                      <span class="text-white heading"><?php $tickets = $_SESSION['tickets'];echo $tickets;?></span>
                      <span class="text-white description">Tickets</span>
                    </div>
                    <div>
                      <span class="text-white heading"><?php $tickets_open = $_SESSION['tickets_open'];echo $tickets_open;?></span>
                      <span class="text-white description">Offene Tickets</span>
                    </div>
                    <div>
                      <span class="text-white heading"><?php $products = $_SESSION['products'];echo $products;?></span>
                      <span class="text-white description">Produkte</span>
                    </div>
                  </div>
                </div>
              </div>
              <div class="text-center">
                <h5 class=" text-white h3">
                <?php $username = $_SESSION['username'];echo $username;?>, <?php $age = $_SESSION['age'];echo $age;?></span>
                </h5>
                <div class="text-white h5 font-weight-300">
                  <i class="ni location_pin mr-2"></i><?php $firstname = $_SESSION['firstname'];echo $firstname;?>, <?php $lastname = $_SESSION['lastname'];echo $lastname;?></span>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-xl-8 order-xl-1">
          <div class="card">
            <div class="card-header">
              <div class="row align-items-center">
                <div class="col-8">
                  <h3 class="text-muted mb-4">Account bearbeiten </h3>
                </div>
            </div>
            <div class="card-body">
                <h6 class="heading-small text-muted mb-4">Kunden Information</h6>
                <div class="pl-lg-4">
                  <div class="row">
                    <div class="col-lg-6">
                      <div class="form-group">
                        <label class="text-white form-control-label" for="input-username">Ram</label>

                        <?php

?>
                        <input type="text" id="input-username" class="form-control" placeholder="<?php echo $row['Ram']?>" value="">
                      </div>
                    </div>
                    <div class="col-lg-6">
                      <div class="form-group">
                        <label class="text-white form-control-label" for="input-email">E-Mail</label>
                        <input type="email" id="input-email" class="form-control" placeholder="<?php echo $row['Cpu']?>">
                      </div>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-lg-6">
                      <div class="form-group">
                        <label class="text-white form-control-label" for="input-first-name">Vorname</label>
                        <input type="text" id="input-first-name" class="form-control" placeholder="<?php echo $row['Speicher']?>" value="">
                      </div>
                    </div>
                    <div class="col-lg-6">
                      <div class="form-group">
                        <label class="text-white form-control-label" for="input-last-name">Speicher?</label>
                        <div class="form-group">
                        <input type="submit" class="btn btn-primary" value="Speichern!">
                </div>
                      </div>
                    </div>
                  </div>
                </div>
                <hr class="my-4" />
                <!-- Address -->
                <h6 class="heading-small text-muted mb-4">Kontakt Information</h6>
                <div class="pl-lg-4">
                  <div class="row">
                    <div class="col-md-12">
                      <div class="form-group">
                        <label class="text-white form-control-label" for="input-address">Straße</label>
                        <input id="input-address" class="form-control" placeholder="<?php $address = $_SESSION['address'];echo $address;?>" value="" type="text">
                      </div>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-lg-4">
                      <div class="form-group">
                        <label class="text-white form-control-label" for="input-city">Stadt</label>
                        <input type="text" id="input-city" class="form-control" placeholder="<?php $city = $_SESSION['city'];echo $city;?>" value="">
                      </div>
                    </div>
                    <div class="col-lg-4">
                      <div class="form-group">
                        <label class="text-white form-control-label" for="input-country">Land</label>
                        <input type="text" id="input-country" class="form-control" placeholder="<?php $country = $_SESSION['country'];echo $country;?>" value="">
                      </div>
                    </div>
                    <div class="col-lg-4">
                      <div class="form-group">
                        <label class="text-white form-control-label" for="input-country">Postleit Zahl</label>
                        <input type="number" id="input-postal-code" class="form-control" placeholder="<?php $postcode = $_SESSION['postcode'];echo $postcode;?>">
                      </div>
                    </div>
                  </div>
                </div>
                <hr class="my-4" />
                <!-- Description -->
                <h6 class="heading-small text-muted mb-4">Passwort Ändern</h6>
                    <form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post"> 
                <div class="form-group <?php echo (!empty($new_password_err)) ? 'has-error' : ''; ?>">
                  <label class="text-white form-control-label">Neues Passwort</label>
                    <input type="password" name="new_password" class="form-control" value="<?php echo $new_password; ?>">
                    <span class="help-block"><?php echo $new_password_err; ?></span>
                </div>
                <div class="form-group <?php echo (!empty($confirm_password_err)) ? 'has-error' : ''; ?>">
                  <label class="text-white form-control-label">Passwort wiederholen</label>
                    <input type="password" name="confirm_password" class="form-control">
                    <span class="help-block"><?php echo $confirm_password_err; ?></span>
                </div>
                <div class="form-group">
                  <input type="submit" class="btn btn-primary" value="Änderung Speichern!">
                </div>

                <h6 class="heading-small text-muted mb-4">Passwort Ändern</h6>
                    <form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post"> 
                <div class="form-group ">
                  <label class="text-white form-control-label">Neues Passwort</label>
                  <input type="text" name="new_ram" class="form-control" value="<?php echo $new_ram; ?>">
                    <span class="help-block"></span>
                </div>
                <div class="form-group">
                  <input type="submit" class="btn btn-primary" value="Änderung Speichern!">
                </div>
              </form>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
  <?php include('../include/main/_chat.php'); ?> 
</body>
    	 <!-- Footer Start -->
       <?php include('../include/main/_footer.php'); ?> 
        <!-- Footer End -->
</html>
