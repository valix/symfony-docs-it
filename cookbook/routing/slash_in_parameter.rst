﻿.. index::
   single: Rotte; Consentire / in un parametro di rotta

Consentire un carattere "/" in un parametro di rotta
====================================================

A volte è necessario comporre URL con parametri che possono contenere una barra
``/``. Per esempio, prendiamo la classica rotta ``/hello/{name}``. Per impostazione predefinita,
``/hello/Fabien`` corrisponderà a questa rotta, ma non ``/hello/Fabien/Kris``. Questo
è dovuto al fatto che Symfony utilizza questo carattere come separatore tra le parti delle rotte.

Questa guida spiega come modificare una rotta in modo che ``/hello/Fabien/Kris``
corrisponda alla rotta ``/hello/{name}``, dove ``{name}`` vale ``Fabien/Kris``.

Configurare la rotta
--------------------

Per impostazione predefinita, il componente delle rotte di symfony richiede che i parametri 
corrispondano alla seguente espressione regolare: ``[^/]+``. Questo significa che tutti i caratteri 
sono permessi eccetto ``/``.

Bisogna consentire esplicitamente che il carattere ``/`` possa far parte del parametro specificando
una espressione regolare più permissiva.

.. configuration-block::

    .. code-block:: php-annotations

        use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;

        class DemoController
        {
            /**
             * @Route("/hello/{name}", name="_hello", requirements={"name"=".+"})
             */
            public function helloAction($name)
            {
                // ...
            }
        }

    .. code-block:: yaml

        _hello:
            path:     /hello/{username}
            defaults: { _controller: AppBundle:Demo:hello }
            requirements:
                username: .+

    .. code-block:: xml

        <?xml version="1.0" encoding="UTF-8" ?>

        <routes xmlns="http://symfony.com/schema/routing"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://symfony.com/schema/routing http://symfony.com/schema/routing/routing-1.0.xsd">

            <route id="_hello" path="/hello/{username}">
                <default key="_controller">AppBundle:Demo:hello</default>
                <requirement key="username">.+</requirement>
            </route>
        </routes>

    .. code-block:: php

        use Symfony\Component\Routing\RouteCollection;
        use Symfony\Component\Routing\Route;

        $collection = new RouteCollection();
        $collection->add('_hello', new Route('/hello/{username}', array(
            '_controller' => 'AppBundle:Demo:hello',
        ), array(
            'username' => '.+',
        )));

        return $collection;

Questo è tutto! Ora, il parametro ``{username}`` può contenere il carattere ``/``.
