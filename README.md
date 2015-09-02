# GoogleDrivePHP
Use API Google drive for upload files

```
composer install
```

Example code

```
<?php
require_once 'vendor/google/apiclient/src/Google/Client.php';
require_once 'vendor/google/apiclient/src/Google/Service/Drive.php';

$client = new Google_Client();
$client->setClientId('<YOUR_CLIENT_ID>');
$client->setClientSecret('<YOUR_CLIENT_SECRET>');
$client->setRedirectUri('<YOUR_REGISTERED_REDIRECT_URI>');
$client->setScopes(array('https://www.googleapis.com/auth/drive.file'));

$session_start();

if (isset($_GET['code']) || (isset($_SESSION['access_token']) && $_SESSION['access_token'])) {
    if (isset($_GET['code'])) {
        $client->authenticate($_GET['code']);
        $_SESSION['access_token'] = $client->getAccessToken();
    } else
        $client->setAccessToken($_SESSION['access_token']);

    $service = new Google_Service_Drive($client);

    $file = new Google_Service_Drive_DriveFile();
    $file->setTitle('image.jpg');
    $file->setDescription('image');
    $file->setMimeType('image/jpeg');

    $data = file_get_contents('image.jpg');

    $createdFile = $service->files->insert($file, array(
          'data' => $data,
          'mimeType' => 'image/jpeg',
          'uploadType' => 'multipart'
        ));

    print_r($createdFile);

} else {
    $authUrl = $client->createAuthUrl();
    header('Location: ' . $authUrl);
    exit();
}

?>
```
