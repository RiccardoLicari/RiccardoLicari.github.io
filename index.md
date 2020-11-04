<?php
date_default_timezone_set('UTC');
if (!empty($_GET['abito'])) 
{
  $servername = "localhost";
  $username = "user";
  $password = "pass";
  $dbname = "I_Sarti_Italiani";
  $minuti=540;
  $flag=false;
  $flagc=false;
  $cucito=60;
  $taglio=30;
  $imbastimento=90;
  $data_scadenza=date(mktime(0, 0, 0, date("m")+1  , date("d") , date("Y")));
  $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
  $dataok=false;
  $datascaduta=false;
  while($data_scadenza>$data_tmp && !$flag)
  {
    $dataok=true;

    if(date("H",$data_tmp)>17)
    {
      $minuti+=900;
      $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
    }
    
    if(date("w",$data_tmp)==6)
    {
      $minuti+=2880;
      $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
    }
    elseif(date("w",$data_tmp)==0)
    {
      $minuti+=1440;
      $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
    }
    else
    {
      echo date("r",  mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")))."<br>";
      $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));

      }




    // Crea connessione
    $conn = new mysqli($servername, $username, $password, $dbname);
    // Controlla Connessione
    if ($conn->connect_error) {
      die("Connection failed: " . $conn->connect_error);
    }

    $sql = "SELECT * FROM operazioni WHERE Date_time_fine >= curdate() AND Descrizione='Taglio' ORDER BY Date_time_inizio ASC " ;
    $result = $conn->query($sql);

    if ($result->num_rows > 0) {
      $i=0;
      // Output dei dati di ogni singola riga
      while(($row = $result->fetch_assoc()) && $dataok) {
        $i++;
        echo "id: " . $row["ID_Operazione"]. " - Descrizione: " . $row["Descrizione"]. " - Date_time_inizio: " . $row["Date_time_inizio"]. " -Data di Fine: ".$row["Date_time_fine"]. "<br>";
        echo date("r", strtotime($row["Date_time_inizio"]))."<br>";
        echo date("r", strtotime($row["Date_time_fine"]))."<br>";
        echo date("r", mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")))."<br>";
        
      
      if(strtotime($row["Date_time_inizio"])-(mktime(0, 0+$minuti+$taglio, 0, date("m")  , date("d") , date("Y")))>=0 || strtotime($row["Date_time_fine"])-mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y"))<=0 )
      {
        $dataok="true";
      }
      else
      {
        $dataok="false";
      break;
      }

    }
    } else {
      echo "0 results";
    }
    $conn->close();
    if($dataok=="true")
    {
      $datascaduta=true;
    break;
    
    }
    $minuti+=30;
  }
  if ($dataok=="true")
  {
   $sql2 = "INSERT INTO `operazioni` (`ID_Operazione`, `Descrizione`, `REF_Blocco_Operazioni`, `REF_Operatore`, `Date_time_inizio`, `Date_time_fine`) VALUES (NULL, 'Taglio', '1', '1',\"".date('Y-m-d H:i:s', $data_tmp)."\" ,\"".date('Y-m-d H:i:s', mktime(0, 0+$minuti+$taglio, 0, date("m")  , date("d") , date("Y")))."\"); " ;

  }
  


  $flag=false;
  $minuti=540;
  $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
  $datascaduta=false;


  while($data_scadenza>$data_tmp && !$flag)
  {
    $dataok=true;

    if(date("H",$data_tmp)>17)
    {
      $minuti+=900;
      $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
    }
    
    if(date("w",$data_tmp)==6)
    {
      $minuti+=2880;
      $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
    }
    elseif(date("w",$data_tmp)==0)
    {
      $minuti+=1440;
      $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
    }
    else
    {
      echo date("r",  mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")))."<br>";
      $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));

      }




    // Crea connessione
    $conn = new mysqli($servername, $username, $password, $dbname);
    // Controlla Connessione
    if ($conn->connect_error) {
      die("Connection failed: " . $conn->connect_error);
    }

    $sql = "SELECT * FROM operazioni WHERE Date_time_fine >= curdate() AND Descrizione='Cucito' ORDER BY Date_time_inizio ASC " ;
    $result = $conn->query($sql);

    if ($result->num_rows > 0) {
      $i=0;
      // Output dei dati di ogni singola riga
      while(($row = $result->fetch_assoc()) && $dataok) {
        $i++;
        echo "id: " . $row["ID_Operazione"]. " - Descrizione: " . $row["Descrizione"]. " - Date_time_inizio: " . $row["Date_time_inizio"]. " -Data di Fine: ".$row["Date_time_fine"]. "<br>";
        echo date("r", strtotime($row["Date_time_inizio"]))."<br>";
        echo date("r", strtotime($row["Date_time_fine"]))."<br>";
        echo date("r", mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")))."<br>";
        
      
      if(strtotime($row["Date_time_inizio"])-(mktime(0, 0+$minuti+$cucito, 0, date("m")  , date("d") , date("Y")))>=0 || strtotime($row["Date_time_fine"])-mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y"))<=0 )
      {
        $dataok="true";
      }
      else
      {
        $dataok="false";
      break;
      }
      

    }
    } else {
      echo "0 results";
    }
    $conn->close();
    if($dataok=="true")
    {
    break;
    }
    $minuti+=30;
  }
  


if($dataok=="true")
{
   $sql3 = "INSERT INTO `operazioni` (`ID_Operazione`, `Descrizione`, `REF_Blocco_Operazioni`, `REF_Operatore`, `Date_time_inizio`, `Date_time_fine`) VALUES (NULL, 'Cucito', '1', '1',\"".date('Y-m-d H:i:s', $data_tmp)."\" ,\"".date('Y-m-d H:i:s', mktime(0, 0+$minuti+$cucito, 0, date("m")  , date("d") , date("Y")))."\"); " ;
}


$flag=false;
$minuti=540;
$data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
$datascaduta=false;


while($data_scadenza>$data_tmp && !$flag)
{
  $dataok=true;

  if(date("H",$data_tmp)>17)
  {
    $minuti+=900;
    $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
  }
  
  if(date("w",$data_tmp)==6)
  {
    $minuti+=2880;
    $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
  }
  elseif(date("w",$data_tmp)==0)
  {
    $minuti+=1440;
    $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));
  }
  else
  {
    echo date("r",  mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")))."<br>";
    $data_tmp=date(mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")));

    }




  // Crea connessione
  $conn = new mysqli($servername, $username, $password, $dbname);
  // Controlla Connessione
  if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
  }

  $sql = "SELECT * FROM operazioni WHERE Date_time_fine >= curdate() AND Descrizione='imbastimento' ORDER BY Date_time_inizio ASC " ;
  $result = $conn->query($sql);

  if ($result->num_rows > 0) {
    $i=0;
    // Output dei dati di ogni singola riga
    while(($row = $result->fetch_assoc()) && $dataok) {
      $i++;
      echo "id: " . $row["ID_Operazione"]. " - Descrizione: " . $row["Descrizione"]. " - Date_time_inizio: " . $row["Date_time_inizio"]. " -Data di Fine: ".$row["Date_time_fine"]. "<br>";
      echo date("r", strtotime($row["Date_time_inizio"]))."<br>";
      echo date("r", strtotime($row["Date_time_fine"]))."<br>";
      echo date("r", mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y")))."<br>";
      
    
    if(strtotime($row["Date_time_inizio"])-(mktime(0, 0+$minuti+$imbastimento, 0, date("m")  , date("d") , date("Y")))>=0 || strtotime($row["Date_time_fine"])-mktime(0, 0+$minuti, 0, date("m")  , date("d") , date("Y"))<=0 )
    {
      $dataok="true";
    }
    else
    {
      $dataok="false";
    break;
    }
    

  }
  } else {
    echo "0 results";
  }
  $conn->close();
  if($dataok=="true")
  {
  break;
  }
  $minuti+=30;
}
if($dataok=="true")
{ $sql4 = "INSERT INTO `operazioni` (`ID_Operazione`, `Descrizione`, `REF_Blocco_Operazioni`, `REF_Operatore`, `Date_time_inizio`, `Date_time_fine`) VALUES (NULL, 'imbastimento', '1', '1',\"".date('Y-m-d H:i:s', $data_tmp)."\" ,\"".date('Y-m-d H:i:s', mktime(0, 0+$minuti+$imbastimento, 0, date("m")  , date("d") , date("Y")))."\"); " ;
}}


// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);
// Check connection
if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}

if ($conn->query($sql2) === TRUE) {
  echo "New record created successfully <br>";
} else {
  echo "Error: " . $sql . "<br>" . $conn->error;
}

if ($conn->query($sql3) === TRUE) {
  echo "New record created successfully <br>";
} else {
  echo "Error: " . $sql . "<br>" . $conn->error;
}

if ($conn->query($sql4) === TRUE) {
  echo "New record created successfully <br>";
} else {
  echo "Error: " . $sql . "<br>" . $conn->error;
}

$conn->close();

?>

<html>

<head></head>

<body>

<form action="index.php" method="get">
  <input type="hidden" name="abito" value="run">
  <input type="submit" value="abito">
</form>




</body>
</html>
