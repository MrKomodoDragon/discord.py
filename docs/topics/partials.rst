:orphan:

.. currentmodule:: discord

.. _guide_partials:

Partials
========

What are partials?
----------------------
Partials are a set of objects that discord.py uses to represent the minimal information to represent the object/entity. Also, partial objects are returned by the library when complete API information wasn't sent by Discord and the library could not resolve the object. For example, :class:`~discord.PartialEmoji` is returned in reaction events such as :func:`~discord.on_reaction_add`, when the library cannot resolve the emoji of the reaction, either because the emoji wasn't in cache or the emoji is in a guild the bot is not a part of.

Why do I want to use partials?
----------------------------------
Partials can be very useful, as you can use them when you have limited information or do not want to make an API call. Certain objects can be constructed by yourself such as :class:`~discord.Object` and :class:`~discord.PartialMessage`. :class:`~discord.Object` in particular is very special. It can be used in a variety of scenarios, as it simply requires a id to create a :class:`~discord.abc.Snowflake`, which is the base class for almost all the Discord models in the library. 

Here is an example of where discord.Object can be used, in a command to sync your :class:`~discord.app_commands.CommandTree` to a guild without fetching it:

.. code-block:: python3
	
	# This sync command was made by Umbra#0009. Thank you Umbra for letting me use it in this example/

	@bot.command()
	@commands.is_owner()
	async def sync(self, ctx: Context, guilds: Greedy[Object], spec: Optional[Literal["~"]] = None) -> None:
	    if not guilds:
	        if spec == "~":
	            fmt = await ctx.bot.tree.sync(guild=ctx.guild)
	        else:
	            fmt = await ctx.bot.tree.sync()

	        await ctx.send(
	            f"Synced {len(fmt)} commands {'globally' if spec is None else 'to the current guild.'}"
	        )
	        return

	    fmt = 0
	    for guild in guilds:
	        try:
	            await ctx.bot.tree.sync(guild=guild)
	        except discord.HTTPException:
	            pass
	        else:
	            fmt += 1

	    await ctx.send(f"Synced the tree to {fmt}/{len(guilds)} guilds.")

In this example, we use :class:`~discord.Object` to convert the guild id to a :class:`~discord.abc.Snowflake` that can be passed to the :meth:`~discord.app_commands.CommandTree.sync` method. This way, we don't have to fetch every single guild, which means we reduce the amount of unnecessary API calls we make. 

Another constructable partial object is :class:`~discord.PartialMessage`. This is also very useful, as you can get a message object to perform operations on without making an API call to fetch the message.

[insert example here]




	