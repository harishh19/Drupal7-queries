<?php
Notes :: Block view alter to render the dynamic menu links based on the entity reference & Og reference. We reuse the code, copy & paste the code check the $block->detla and other related values

/*
 * @fie Common module provides the common functionalities to all modules.
 */
/*
 * Implements hook_block_view_alter().
 */

function common_block_view_alter(&$data, $block) {
  $arg = arg();
  if ($block->delta == "menu-about-course") {
    $arg = arg();
    if ($arg[0] == 'aboutcourse' && is_numeric($arg[1])) {
      dsm($data['content']);
      $ref_array = array();
      $output = array();
      $sorted = $data['content']['#sorted'];
      $theme_wrapper = $data['content']['#theme_wrappers'];
      $contextual_links = $data['content']['#contextual_links'];
      foreach ($data['content'] as $parent_menuId => $menuDetails) {
        $output[$parent_menuId] = $menuDetails;
        if (!empty($menuDetails['#href']) && !empty($menuDetails['#below'])) {
          foreach ($menuDetails['#below'] as $sub_menuId => $subMenuDetails) {
            if (!empty($subMenuDetails['#href']) && !empty($subMenuDetails['#original_link']['href'])) {
              $entity_id = explode('/', $subMenuDetails['#original_link']['href']);
              $data_links = _get_entity_reference_group_check($entity_id[1], $arg[1]);
              if (!empty($data_links)) {
                $output[$parent_menuId]['#below'][$sub_menuId] = $subMenuDetails;
              }
              else {
                $output[$parent_menuId]['#below'][$sub_menuId] = "";
              }
            }
          }
        }
      }
      $data['content'] = $output;
      $data['content']['#sorted'] = $sorted;
      $data['content']['#theme_wrappers'] = $theme_wrapper;
      $data['content']['#contextual_links'] = $contextual_links;
    }
  }
}

/**
 * Get the nid if present viewing group associated to render data.
 * 
 * @param type $nid
 * @param type $groupId
 * @return type
 */
function _get_entity_reference_group_check($nid, $groupId) {
  $query = db_select('node', 'n');
  $query->leftjoin('og_membership', 'ogr', 'ogr.etid = n.nid');
  $query->leftjoin('field_data_ field_course_name', 'fcn', 'fcn.entity_id=n.nid');
  $query->fields('n', array('nid', 'title'));
  $query->condition('n.nid', $nid);
  // Check condition with og group content or default entity types.
  $or = db_or();
  $or->condition('ogr.gid', $groupId);
  $or->condition('fcn.field_course_name_target_id', $groupId);
  $query->condition($or);
  $result = $query->execute()->fetchAll();
  return $result;
}
