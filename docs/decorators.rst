.. _decorators:

Built-in decorators
===================


Allowed users::

    from mattermost_bot.utils import allowed_users
    from mattermost_bot.bot import respond_to


    @respond_to('^is-allowed$')
    @allowed_users('admin', 'root')
    def access_allowed(message):
        message.reply('Access is allowed')



Direct messages::

    from mattermost_bot.utils import allow_only_direct_message
    from mattermost_bot.bot import respond_to


    @respond_to('^direct-msg$')
    @allow_only_direct_message()
    def direct_msg(message):
        message.reply('Message is direct')



Real case
---------

For example we have some users on our chat. We want allow some functionality
to some users. We can create constants with allowed users on bot settings::

    ADMIN_USERS = ['admin', 'root']
    SUPPORT_USERS = ['mike', 'nick']


So now we can close access to some functions on plugins::

    from mattermost_bot.utils import allow_only_direct_message
    from mattermost_bot.utils import allowed_users
    from mattermost_bot.bot import respond_to
    from mattermost_bot import settings


    @respond_to('^selfupdate')
    @allow_only_direct_message()
    @allowed_users(*settings.ADMIN_USERS)
    def self_update(message):
        # some code
        message.reply('Update was done')


    @respond_to('^smsto (.*)')
    @allowed_users(*settings.SUPPORT_USERS)
    def sms_to(message, name):
        # some code
        message.reply('Sms was sent to %s' % name)
