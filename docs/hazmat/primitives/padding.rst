.. hazmat::

Padding
=======

.. currentmodule:: cryptography.hazmat.primitives.padding

Padding is a way to take data that may or may not be be a multiple of the block
size for a cipher and extend it out so that it is. This is required for many
block cipher modes as they require the data to be encrypted to be an exact
multiple of the block size.


.. class:: PKCS7(block_size)

    PKCS7 padding is a generalization of PKCS5 padding (also known as standard
    padding). PKCS7 padding works by appending ``N`` bytes with the value of
    ``chr(N)``, where ``N`` is the number of bytes required to make the final
    block of data the same size as the block size. A simple example of padding
    is:

    .. doctest::

        >>> from cryptography.hazmat.primitives import padding
        >>> padder = padding.PKCS7(128).padder()
        >>> padder.update(b"1111111111")
        ''
        >>> padded_data = padder.finalize()
        >>> padded_data
        '1111111111\x06\x06\x06\x06\x06\x06'
        >>> unpadder = padding.PKCS7(128).unpadder()
        >>> unpadder.update(padded_data)
        ''
        >>> unpadder.finalize()
        '1111111111'

    :param block_size: The size of the block in bits that the data is being
                       padded to.

    .. method:: padder()

        :returns: A padding
            :class:`~cryptography.hazmat.primitives.interfaces.PaddingContext`
            provider.

    .. method:: unpadder()

        :returns: An unpadding
            :class:`~cryptography.hazmat.primitives.interfaces.PaddingContext`
            provider.


.. currentmodule:: cryptography.hazmat.primitives.interfaces

.. class:: PaddingContext

    When calling ``padder()`` or ``unpadder()`` you will receive an a return
    object conforming to the ``PaddingContext`` interface. You can then call
    ``update(data)`` with data until you have fed everything into the context.
    Once that is done call ``finalize()`` to finish the operation and obtain
    the remainder of the data.

    .. method:: update(data)

        :param bytes data: The data you wish to pass into the context.
        :return bytes: Returns the data that was padded or unpadded.

    .. method:: finalize()

        :return bytes: Returns the remainder of the data.
