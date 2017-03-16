Note :: working guzzlehttps request,creating new http request. Copy&paste check the "autoload.php" path, need to send params on _construct function.

<?php

/**
 * @file
 * Method include file for 'Application Integration with POBT'.
 */
require_once DRUPAL_ROOT . '/profiles/apigee/libraries/mgmt-api-php-sdk/vendor/autoload.php';

use Guzzle\Http\Client;
use Guzzle\Http\EntityBody;
use Guzzle\Http\Message\Request;
use Guzzle\Http\Message\Response;

/**
 * Method class for initiate POBT API.
 */
class GarminProgramAppIntegration {

  /**
   * The client object as an instance of
   * {@link http://api.guzzlephp.org/class-Guzzle.Http.Client.html \Guzzle\Http\Client}.
   *
   * @var \Guzzle\Http\Client
   */
  public $client;

  /**
   * @var success;
   */
  public $success;

  /**
   * Initializes the OrgConfig for this class.
   *
   * @param string $base_url
   */
  public function __construct($base_url, $post_uri, $data) {
    try {
      $client = new Client($base_url);
      $request = $client->post($post_uri, array('Content-Type' => 'application/x-www-form-urlencoded'), $data);
      $response = $request->send();
      $statusCode = $response->getStatusCode();
      if ($statusCode == 200) {
        $this->success = 'true';
      }
    }
    catch (Exception $e) {
      $apiError_message = t("Something went Wrong with provided API and params. Http error message :: ( @message ).", array('@message' => $e->getMessage()));
      watchdog(WATCHDOG_ERROR, $apiError_message);
      drupal_set_message($apiError_message, 'error');
      $this->success = 'false';
    }
  }

}

