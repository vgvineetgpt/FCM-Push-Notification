# FCM-Push-Notification
How to Implement FCM In PHP for both IOS and Android


<?php

 function sendFCM($body, $title, $id, $StudentAdmissionId, $activitytype) {
        $url = "https://fcm.googleapis.com/fcm/send";
        $token = $id;
        $serverKey = 'AAAA8XYL2Y8:APA91bF1fsddfgffdssdsdfsdsdsda8zyWJTgs0OSeiRlk9WQqLwKn51VkMH_XSbpRuiCTU-Fdi2hoV8JY8ST7gCBQe4dlMFASDbr5Oci1bLmg9tyl5dlxyFDauWWCMItUNdFGWO_CWhpHwSIbvbqYwlCnoRd7ucB';
        $notification = array(
            'title' => $title,
            'text' => $body,
            'sound' => 'default',
            'badge' => 1,
            'category' => $activitytype,
            'content-available' => 1
        );
        $arrayToSend = array(
            'to' => $token,
            'notification' => $notification,
            'priority' => 'high'
        );
        $json = json_encode($arrayToSend);
        $headers = array();
        $headers[] = 'Content-Type: application/json';
        $headers[] = 'Authorization: key=' . $serverKey;
        $ch = curl_init();

        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
        curl_setopt($ch, CURLOPT_POSTFIELDS, $json);
        curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
        //Send the request
        echo $response = curl_exec($ch);
        //Close request
        if ($response === FALSE) {
            die('FCM Send Error: ' . curl_error($ch));
        }
        curl_close($ch);
        mysql_query("INSERT INTO `tbl_notifications`(`n_activity_name`,`n_user_id`,`n_device_id`, `n_notification`) VALUES ('$activitytype','$StudentAdmissionId','$id','$response')");
        // #Echo Result Of FireBase Server
        return $response;
    }

?>
