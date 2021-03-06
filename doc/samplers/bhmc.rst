.. index:: Sampling Functions; Binary Hamiltonian Monte Carlo

.. _section-BHMC:

Binary Hamiltonian Monte Carlo (BHMC)
-------------------------------------

Implementation of the binary-state Hamiltonian Monte Carlo sampler of Pakman :cite:`pakman:2013:hmcb`.  The sampler simulates autocorrelated draws from a distribution that can be specified up to a constant of proportionality.


Stand-Alone Function
^^^^^^^^^^^^^^^^^^^^

.. function:: bhmc!(v::BHMCVariate, traveltime::Real, logf::Function)

    Simulate one draw from a target distribution using the BHMC sampler.  Parameters are assumed to have binary numerical values (0 or 1).

    **Arguments**

        * ``v`` : current state of parameters to be simulated.
        * ``traveltime`` : length of time over which particle paths are simulated.  It is recommended that supplied values be of the form :math:`(n + \frac{1}{2}) \pi`, where optimal choices of :math:`n \in \mathbb{Z}^+` are expected to grow with the parameter space dimensionality.
        * ``logf`` : function that takes a single ``DenseVector`` argument of parameter values at which to compute the log-transformed density (up to a normalizing constant).

    **Value**

        Returns ``v`` updated with simulated values and associated tuning parameters.

    .. _example-bhmc:

    **Example**

        .. literalinclude:: bhmc.jl
            :language: julia


.. index:: Sampler Types; BHMCVariate

BHMCVariate Type
^^^^^^^^^^^^^^^^

Declaration
```````````

``BHMCVariate <: VectorVariate``

Fields
``````

* ``value::Vector{Float64}`` : vector of sampled values.
* ``tune::BHMCTune`` : tuning parameters for the sampling algorithm.

Constructors
````````````

.. function:: BHMCVariate(x::AbstractVector{T<:Real}, tune::BHMCTune)
              BHMCVariate(x::AbstractVector{T<:Real}, tune=nothing)

    Construct a ``BHMCVariate`` object that stores sampled values and tuning parameters for BHMC sampling.

    **Arguments**

        * ``x`` : vector of sampled values.
        * ``tune`` : tuning parameters for the sampling algorithm.  If ``nothing`` is supplied, parameters are set to their defaults.

    **Value**

        Returns a ``BHMCVariate`` type object with fields pointing to the values supplied to arguments ``x`` and ``tune``.

.. index:: Sampler Types; BHMCTune

BHMCTune Type
^^^^^^^^^^^^^

Declaration
```````````

``type BHMCTune``

Fields
``````
* ``traveltime::Float64`` : length of time over which particle paths are simulated.
* ``position::Vector{Float64}`` : initial particle positions.
* ``velocity::Vector{Float64}`` : initial particle velocites.
* ``wallhits::Int`` : number of times particles are reflected off the 0 threshold.
* ``wallcrosses::Int`` : number of times particles travel through the threshold.

Sampler Constructor
^^^^^^^^^^^^^^^^^^^

.. function:: BHMC(params::Vector{Symbol}, traveltime::Real)

    Construct a ``Sampler`` object for BHMC sampling.  Parameters are assumed to have binary numerical values (0 or 1).

    **Arguments**

        * ``params`` : stochastic nodes containing the parameters to be updated with the sampler.
        * ``traveltime`` : length of time over which particle paths are simulated.  It is recommended that supplied values be of the form :math:`(n + \frac{1}{2}) \pi`, where optimal choices of :math:`n \in \mathbb{Z}^+` are expected to grow with the parameter space dimensionality.

    **Value**

        Returns a ``Sampler`` type object.

    **Example**

        See the :ref:`Pollution <example-Pollution>` and other :ref:`section-Examples`.
