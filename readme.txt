##
##
##        Mod title:  View Own Profile(while logged in)
##
##      Mod version:  1.0
##  Works on FLuxBB:  1.5.3
##     Release date:  Do Not Know Yet :)
##           Author:  Daniël Rokven (kierownik) rokven@gmail.com
##  Original Author:  Frank Smit (FSX) FSX.NR01@gmail.com for Punbb
##
##      Description:  You can see your profile like other 
##                    people see your profile (not in edit mode).
##
##       Affects DB:  No
##
##            Notes:  Found this mod on punres.net and converted it for fluxbb 1.5.3
##                    http://www.punres.net/desc.php?pid=342
##
##   Affected files:  profile.php
##                    lang/English/profile.php
##                    include/functions.php
##
##       DISCLAIMER:  Please note that 'mods' are not officially supported by
##                    FluxBB. Installation of this modification is done at your
##                    own risk. Backup your forum database and any and all
##                    applicable files before proceeding.
##

#
#---------[ 1. OPEN ]---------------------------------------------------------
#

profile.php

#
#---------[ 2. FIND (line: 1686) ]--------------------------------------------
#

	else if ($section == 'admin')

#
#---------[ 3. BEFORE, ADD ]--------------------------------------------------
#

	// Begin View Own Profile mod
		else if ($section == 'viewownprofile')
	{
		$page_title = array(pun_htmlspecialchars($pun_config['o_board_title']).' / '.$lang_profile['title view own profile']);
		define('PUN_ACTIVE_PAGE', 'profile');
		require PUN_ROOT.'header.php';

		generate_profile_menu('example');

	if ($user['email_setting'] == '0' && !$pun_user['is_guest'])
		$email_field = '<a href="mailto:'.$user['email'].'">'.$user['email'].'</a>';
	else if ($user['email_setting'] == '1' && !$pun_user['is_guest'])
		$email_field = '<a href="misc.php?email='.$id.'">'.$lang_common['Send email'].'</a>';
	else
		$email_field = $lang_profile['Private'];

	$user_title_field = get_title($user);

	if ($user['url'] != '')
	{
		$user['url'] = pun_htmlspecialchars($user['url']);

		if ($pun_config['o_censoring'] == '1')
			$user['url'] = censor_words($user['url']);

		$url = '<a href="'.$user['url'].'">'.$user['url'].'</a>';
	}
	else
		$url = $lang_profile['unknown'];

	if ($pun_config['o_avatars'] == '1')
	{
		if ($img_size = @getimagesize($pun_config['o_avatars_dir'].'/'.$id.'.gif'))
			$avatar_field = '<img src="'.$pun_config['o_avatars_dir'].'/'.$id.'.gif" '.$img_size[3].' alt="" />';
		else if ($img_size = @getimagesize($pun_config['o_avatars_dir'].'/'.$id.'.jpg'))
			$avatar_field = '<img src="'.$pun_config['o_avatars_dir'].'/'.$id.'.jpg" '.$img_size[3].' alt="" />';
		else if ($img_size = @getimagesize($pun_config['o_avatars_dir'].'/'.$id.'.png'))
			$avatar_field = '<img src="'.$pun_config['o_avatars_dir'].'/'.$id.'.png" '.$img_size[3].' alt="" />';
		else
			$avatar_field = $lang_profile['unknown'];
	}

	$posts_field = '';
	if ($pun_config['o_show_post_count'] == '1' || $pun_user['g_id'] < PUN_GUEST)
		$posts_field = $user['num_posts'];
	if ($pun_user['g_search'] == '1')
		$posts_field .= (($posts_field != '') ? ' - ' : '').'<a href="search.php?action=show_user&amp;user_id='.$id.'">'.$lang_profile['Show posts'].'</a>';

?>

<div id="viewprofile" class="block">
	<h2><span><?php echo $lang_common['Profile'] ?> - <small><?php echo $lang_profile['others see'] ?></small></span></h2>
	<div class="box">
		<div class="fakeform">
			<div class="inform">
				<fieldset>
				<legend><?php echo $lang_profile['Section personal'] ?></legend>
					<div class="infldset">
						<dl>
							<dt><?php echo $lang_common['Username'] ?>: </dt>
							<dd><?php echo pun_htmlspecialchars($user['username']) ?></dd>
							<dt><?php echo $lang_common['Title'] ?>: </dt>
							<dd><?php echo ($pun_config['o_censoring'] == '1') ? censor_words($user_title_field) : $user_title_field; ?></dd>
							<dt><?php echo $lang_profile['Realname'] ?>: </dt>
							<dd><?php echo ($user['realname'] !='') ? pun_htmlspecialchars(($pun_config['o_censoring'] == '1') ? censor_words($user['realname']) : $user['realname']) : $lang_profile['unknown']; ?></dd>
							<dt><?php echo $lang_profile['Location'] ?>: </dt>
							<dd><?php echo ($user['location'] !='') ? pun_htmlspecialchars(($pun_config['o_censoring'] == '1') ? censor_words($user['location']) : $user['location']) : $lang_profile['unknown']; ?></dd>
							<dt><?php echo $lang_profile['Website'] ?>: </dt>
							<dd><?php echo $url ?>&nbsp;</dd>
							<dt><?php echo $lang_profile['email'] ?>: </dt>
							<dd><?php echo $email_field ?></dd

						</dl>
						<div class="clearer"></div>
					</div>
				</fieldset>
			</div>
			<div class="inform">
				<fieldset>
				<legend><?php echo $lang_profile['Section messaging'] ?></legend>
					<div class="infldset">
						<dl>
							<dt><?php echo $lang_profile['Jabber'] ?>: </dt>
							<dd><?php echo ($user['jabber'] !='') ? pun_htmlspecialchars($user['jabber']) : $lang_profile['unknown']; ?></dd>
							<dt><?php echo $lang_profile['ICQ'] ?>: </dt>
							<dd><?php echo ($user['icq'] !='') ? $user['icq'] : $lang_profile['unknown']; ?></dd>
							<dt><?php echo $lang_profile['MSN'] ?>: </dt>
							<dd><?php echo ($user['msn'] !='') ? pun_htmlspecialchars(($pun_config['o_censoring'] == '1') ? censor_words($user['msn']) : $user['msn']) : $lang_profile['unknown']; ?></dd>
							<dt><?php echo $lang_profile['AOL IM'] ?>: </dt>
							<dd><?php echo ($user['aim'] !='') ? pun_htmlspecialchars(($pun_config['o_censoring'] == '1') ? censor_words($user['aim']) : $user['aim']) : $lang_profile['unknown']; ?></dd>
							<dt><?php echo $lang_profile['Yahoo'] ?>: </dt>
							<dd><?php echo ($user['yahoo'] !='') ? pun_htmlspecialchars(($pun_config['o_censoring'] == '1') ? censor_words($user['yahoo']) : $user['yahoo']) : $lang_profile['unknown']; ?></dd>
						</dl>
						<div class="clearer"></div>
					</div>
				</fieldset>
			</div>
			<div class="inform">
				<fieldset>
				<legend><?php echo $lang_profile['Section personality'] ?></legend>
					<div class="infldset">
						<dl>
<?php if ($pun_config['o_avatars'] == '1'): ?>							<dt><?php echo $lang_profile['Avatar'] ?>: </dt>
							<dd><?php echo $avatar_field ?></dd>
<?php endif; ?>							<dt><?php echo $lang_profile['Signature'] ?>: </dt>
							<dd><div><?php echo isset($parsed_signature) ? $parsed_signature : $lang_profile['No sig']; ?></div></dd>
						</dl>
						<div class="clearer"></div>
					</div>
				</fieldset>
			</div>
			<div class="inform">
				<fieldset>
				<legend><?php echo $lang_profile['User activity'] ?></legend>
					<div class="infldset">
						<dl>
<?php if ($posts_field != ''): ?>							<dt><?php echo $lang_common['Posts'] ?>: </dt>
							<dd><?php echo $posts_field ?></dd>
<?php endif; ?>							<dt><?php echo $lang_common['Last post'] ?>: </dt>
							<dd><?php echo $last_post ?></dd>
							<dt><?php echo $lang_common['Registered'] ?>: </dt>
							<dd><?php echo format_time($user['registered'], true) ?></dd>
						</dl>
						<div class="clearer"></div>
					</div>
				</fieldset>
			</div>
		</div>
	</div>
</div>

<?php

	}
	// End View Own Profile mod

#
#---------[ 4. OPEN ]---------------------------------------------------------
#

lang/English/profile.php

#
#---------[ 5. FIND (line: 142) ]---------------------------------------------
#

);

#
#---------[ 6. BEFORE, ADD ]--------------------------------------------------
#

// Begin View Own Profile mod
'section view own profile'  =>  'View Own Profile',
'title view own profile'    =>  'View Profile',
'others see'                =>  'This is what others see when they view your profile',
'private'                   =>  'Private',
'unknown'                   =>  'Unknown',
'email'                     =>  'E-mail',
// End View Own Profile mod

#
#---------[ 7. OPEN ]---------------------------------------------------------
#

iclude/functions.php

#
#---------[ 8. FIND (line: 526) ]---------------------------------------------
#

					<li<?php if ($page == 'privacy') echo ' class="isactive"'; ?>><a href="profile.php?section=privacy&amp;id=<?php echo $id ?>"><?php echo $lang_profile['Section privacy'] ?></a></li>

#
#---------[ 9. AFTER, ADD ]---------------------------------------------------
#

<!-- Begin View Own Profile mod -->
					<li<?php if ($page == 'viewownprofile') echo ' class="isactive"'; ?>><a href="profile.php?section=viewownprofile&amp;id=<?php echo $id ?>"><?php echo $lang_profile['section view own profile'] ?></a></li>
<!-- End View Own Profile mod -->

#
#---------[ 10. SAVE/UPLOAD ]-------------------------------------------------
#